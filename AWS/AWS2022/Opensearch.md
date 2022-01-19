title: Customize your AWS Well-Architected review
date: 2022-01-11 10:37:23
tags:
- AWS
- OpenSearch
---


## Elastic Search Multi-tenancy

https://youtu.be/95kQkS51VnU

## Elastic Search Security

https://youtu.be/B7EiYjtQQ_c

* Access
* Authentication
* Authorization
* Audit

### How ES security works

* ES terminology : Users, Permissions (like IAM Policy), Roles
* Mapping : backend roles (SAML, IAM, ADFS, etc) --> ES Roles

This is the foundation of how multi-tenancy works
* allows multiple teams share the same ES cluster (but have different access to indexes)


## Opensearch

https://youtu.be/y7cp_5Lv2A4

## Pintrest
* Reference architecture for fine-grained log access for k8s logs
   * using Rego Policy declaration
   * Sync Rego and OpenSearch FGAC
   * SAML to Okta to access Opensearch
