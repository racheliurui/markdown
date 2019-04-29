title: AWS - Notes about SSO with Azure
date: 2019-02-06 08:46:00
tags:
- AWS
- Azure
- SSO
---

# Config Azure AD SSO to AWS Console via SMAL

## Azure Official Doc
https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/amazon-web-service-tutorial


## Aditional Notes

The config not align with above doc but needed when doing the config,

Example of claim key/values:

* name: emailaddress
* Namespace: http://schemas.xmlsoap.org/ws/2005/05/identity/claims
* Source: Attribute
* Source attribute: user.mail

Full config as below


```config
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
user.mail

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
user.givenname

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
user.userprincipalname

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
user.userprincipalname

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname
user.surname

https://aws.amazon.com/SAML/Attributes/Role
user.assignedroles

https://aws.amazon.com/SAML/Attributes/RoleSessionName
user.userprincipalname

```


After successful config, login via
https://account.activedirectory.windowsazure.com/r#/applications
