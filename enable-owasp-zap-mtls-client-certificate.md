---
description: >-
  How to set client certificate if you need to test API with mutual
  authentication enabled
---

# Enable OWASP ZAP mTLS/Client Certificate

Well, it's easy. OWASP ZAP supports client certificate so we don't need to take extra actions. Go to Preference -&gt; Client Certificate, check "Enable unsafe SSL/TLS renegotiaion" if you use a self signed cert, check "Use client certificate" to add your cert. 

![](.gitbook/assets/image%20%2820%29.png)



