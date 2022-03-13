---
description: Examples of missed code checks that could cause sever security issues
---

# Security Bugs / Incidents

### Summary

#### Deserialization

Avoid deserialization of untrusted data. And if not, well-crafted whitelists containing only primitives or classes that contain only primitives are a solid way to avoid deserialization issues.
