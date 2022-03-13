---
description: >-
  OpenAPI XML type validator created XML parsers without taking appropriate
  defense against
---

# XML Parsing Eclipse Vert.X (XXE)

Recently there was a security issue found in the version from 3.5.Beta1 to 3.5.3 of Eclipse Vert.x, where the OpenAPI XML type validator created XML parsers without taking appropriate defense against.

> XXE - XML External Entitiy Injection Attack

### Vulnerability

```java
// XMLTypeValidator.java
 
public class XMLTypeValidator implements ParameterTypeValidator {
 
  private Validator schemaValidator;
 
  private XMLTypeValidator(Validator schemaValidator) {
    this.schemaValidator = schemaValidator;
  }
 
  @Override
  public RequestParameter isValid(String value) throws ValidationException {
    try {
      DocumentBuilder parser = DocumentBuilderFactory.newInstance().newDocumentBuilder();
      Document document = parser.parse(value);
      this.schemaValidator.validate(new DOMSource(document));
      return RequestParameter.create(document);
    } catch (Exception e) {
      throw ValidationException.ValidationExceptionFactory.generateInvalidXMLBodyException(e.getMessage());
    }
  }
 
  public static class XMLTypeValidatorFactory {
    public static XMLTypeValidator createXMLTypeValidator(String xmlSchema) {
      // create a SchemaFactory capable of understanding WXS schemas
      SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
 
      // load a WXS schema, represented by a Schema instance
      Source xmlSchemaSource = new StreamSource(new StringReader(xmlSchema));
      try {
        return new XMLTypeValidator(factory.newSchema(xmlSchemaSource).newValidator());
      } catch (SAXException e) {
        e.printStackTrace();
        return null;
      }
    }
  }
 
}

```

**Line 14**

The XXE vulnerability is triggered when a string is parsed by the `parser.parse(value)`. `DocumentBuilderFactory` is not securely configured and allows the processing of the external document type definition.\
\
`DocumentBuilderFactory` class (alongside with many other Java classes that parse XML) allows setting features granularly using the `setFeature()` method. \
\
Four features that protect against XXE attacks: ([full features](https://xerces.apache.org/xerces2-j/features.html))

* [http://apache.org/xml/features/disallow-doctype-decl](http://apache.org/xml/features/disallow-doctype-decl) - completely disallows the declaration of DOCTYPE
* [http://xml.org/sax/features/external-general-entities](http://xml.org/sax/features/external-general-entities) - allows the inclusion of the external general entities
* [http://xml.org/sax/features/external-parameter-entities](http://xml.org/sax/features/external-parameter-entities) - allows inclusion of the external parameter entities
* [http://apache.org/xml/features/nonvalidating/load-external-dtd](http://apache.org/xml/features/nonvalidating/load-external-dtd) - allows loading the external Document Type Definitions (DTD)

Note that these features are off by default, thus it is crucial to set up an XML parser correctly before parsing XML.

**Line 26**

The second XXE vulnerability occurs when an instance of `XMLTypeValidator` class is created using a `SchemaFactory` without secure settings. The `SchemaFactory` class allows setting two properties that were designed to protect from XXE:

* [ACCESS\_EXTERNAL\_DTD](https://docs.oracle.com/javase/7/docs/api/javax/xml/XMLConstants.html#ACCESS\_EXTERNAL\_DTD) - Restrict access to external DTDs and external Entity References to the protocols specified
* [ACCESS\_EXTERNAL\_SCHEMA](https://docs.oracle.com/javase/7/docs/api/javax/xml/XMLConstants.html#ACCESS\_EXTERNAL\_SCHEMA) - Restrict access to the protocols specified for an external reference set by the schemaLocation attribute, Import and Include element

### Fix

Vert.x developers have followed OWASP anti-XXE guidelines. They added `createDocumentBuilderFactoryInstance()` and `createSchemaFactoryInstance()` methods that create instances of `DocumentBuilderFactory` and `SchemaFactory` respectively with the necessary security settings.

Diff: [https://github.com/vert-x3/vertx-web/commit/ac8692c618d6180a9bc012a2ac8dbec821b1a973?branch=ac8692c618d6180a9bc012a2ac8dbec821b1a973\&diff=split](https://github.com/vert-x3/vertx-web/commit/ac8692c618d6180a9bc012a2ac8dbec821b1a973?branch=ac8692c618d6180a9bc012a2ac8dbec821b1a973\&diff=split)

```java
// XMLTypeValidator.java
 
private static DocumentBuilderFactory createDocumentBuilderFactoryInstance() throws ParserConfigurationException {
 
    final DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
    String FEATURE;
 
    FEATURE = "http://apache.org/xml/features/disallow-doctype-decl";
    dbf.setFeature(FEATURE, true);
 
    FEATURE = "http://xml.org/sax/features/external-general-entities";
    dbf.setFeature(FEATURE, false);
 
    FEATURE = "http://xml.org/sax/features/external-parameter-entities";
    dbf.setFeature(FEATURE, false);
 
    FEATURE = "http://apache.org/xml/features/nonvalidating/load-external-dtd";
    dbf.setFeature(FEATURE, false);
 
    dbf.setXIncludeAware(false);
    dbf.setExpandEntityReferences(false);
 
    return dbf;
  }
 
private static SchemaFactory createSchemaFactoryInstance() throws SAXNotRecognizedException, SAXNotSupportedException {
    final SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
    factory.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
    factory.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");
 
    return factory;
  }
```

### Learnings

> This issue is not affected by user/application input, but only by badly crafted application configuration. Users using the API contracts module with the default settings are not affected, only if explicitly enabled the XML validation and provided a malicious XSD as input. \
> \- According to the comment of one of the Vert.X developers

Nevertheless, XXE is a dangerous vulnerability that may lead to the disclosure of the sensitive data on the server, DDOS, a further SSRF attack, etc.

### Useful resources:

* [https://owasp.org/www-community/vulnerabilities/XML\_External\_Entity\_(XXE)\_Processing](https://owasp.org/www-community/vulnerabilities/XML\_External\_Entity\_\(XXE\)\_Processing)
*

