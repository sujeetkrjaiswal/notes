---
description: >-
  SessionData object was deserialized without checking the object types, and
  that opened possibilities for exploitation
---

# Unchecked Deserialization

Recently there was a security issue found in Pippo framework v.1.11.0 that dealt with deserialization: a SessionData object was deserialized without checking the object types, and that opened possibilities for exploitation. For example, some attacker could create a malicious object, base64-encode it, and place into the cookie. Sending this cookie will then lead to the remote code execution.

### Vulnerability

Vulnerable Code: [https://github.com/pippo-java/pippo/blob/233c515476ccfba9d4080a19a7f73c1dad22d248/pippo-session-parent/pippo-session/src/main/java/ro/pippo/session/SerializationSessionDataTranscoder.java](https://github.com/pippo-java/pippo/blob/233c515476ccfba9d4080a19a7f73c1dad22d248/pippo-session-parent/pippo-session/src/main/java/ro/pippo/session/SerializationSessionDataTranscoder.java)

```
/*
 * Copyright (C) 2015 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
package ro.pippo.session;

import ro.pippo.core.PippoRuntimeException;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.util.Base64;

/**
 * A {@link SessionDataTranscoder} that serializes {@link SessionData}s using
 * java serialization.
 *
 * @author Decebal Suiu
 */
public class SerializationSessionDataTranscoder implements SessionDataTranscoder {

    @Override
    public String encode(SessionData sessionData) {
        try (ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
                ObjectOutputStream objectOutputStream = new ObjectOutputStream(outputStream)) {
            objectOutputStream.writeObject(sessionData);
            byte[] bytes = outputStream.toByteArray();
            return Base64.getEncoder().encodeToString(bytes);
        } catch (IOException e) {
            throw new PippoRuntimeException(e);
        }
    }

    @Override
    public SessionData decode(String data) {
        byte[] bytes = Base64.getDecoder().decode(data);
        try (ByteArrayInputStream inputStream = new ByteArrayInputStream(bytes);
                ObjectInputStream objectInputStream = new ObjectInputStream(inputStream)) {
            return (SessionData) objectInputStream.readObject();
        } catch (IOException | ClassNotFoundException e) {
            throw new PippoRuntimeException(e, "Cannot deserialize session. A new one will be created.");
        }
    }

}
```

**Explanation of line with issue (Line 52):**\
The vulnerability is triggered in the `decode(String data)` method when the `objectInputStream.readObject()` is called. `objectInputStream.readObject()` reads an object from the unsanitized `objectInputStream` is derived from a user-supplied cookie. If the cookie is tainted and contains a malicious set of nested objects (a gadget chain), then deserialization of this cookie using the `objectInputStream.readObject()` can lead to the remote command execution.\
\
`ObjectInputStream` resolves any class that it gets as an input, even if there are nested objects of different classes. `ObjectInputStream` doesn't have any built-in security mechanisms that prevent the deserialization of dangerous arbitrary objects. Also, no custom security controls were used in this case. Thus, if `ObjectinputStream` receives a complex nested object, it easily deserializes it into the objects of known classes. JAVA's default set of libraries contains a decent amount of classes that can be used in gadget chains and trigger the remote code execution via the deserialization.

### Fix

If the class of the object from the input stream is not whitelisted, the serialization is aborted. This technique successfully prevents the remote command execution if the whitelist contains the minimal amount of classes that are allowed to be deserialized and there is no gadget chain that uses only whitelisted classes.\
\
Malicious gadgets are nested objects that, when deserialized, result in the execution of an OS command by creating, for example, `ProcessBuilder` object. Thus, whitelisting deserializable classes allows avoiding the OS command execution by breaking the gadget chain and not serializing `ProcessBuilder` class object.\
\
Diff of the fix: [https://github.com/pippo-java/pippo/commit/a82347d9d3358e98c89b48579d4285d807a57cc0](https://github.com/pippo-java/pippo/commit/a82347d9d3358e98c89b48579d4285d807a57cc0)



The suggested whitelist contains `HashMap` and `ArrayList` classes. Even these benign classes still can be used to attack the application. `ObjectInputStream` allows to deserialize nested objects, thus, HashMap of HashMap of HashMap and so on can bypass the whitelist. Specially crafted nested HashMaps lead to the heap overflow in Java Virtual Machine. Multiple requests to the application with the malicious input consume the significant amount of server resources and lead to the Denial of Service.\
\
A reasonable way to mitigate the deserialization issues with `ObjectInputStream` is to avoid using `ObjectInputStream` with the arbitrary user input and start using a more secure deserialization library. And the best way to avoid deserialization issues is to completely avoid deserialization of arbitrary data that is received from the user.

### **Learning:**

\
The best way to avoid deserialization issues is to avoid the deserialization of untrusted data at all. But often this recommendation doesn't fit because the deserialization is deeply rooted in the software architecture, and the only way to avoid the deserialization is to rewrite the part of the application from scratch. Deserialization is also so fast and efficient that there are no viable alternatives to it. Anyway, if you are on the software modeling stage, try to avoid the deserialization of untrusted arbitrary data as a part of the overall design. If it is not possible, then well-crafted whitelists containing only primitives or classes that contain only primitives are a solid way to avoid deserialization issues.
