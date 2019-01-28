title: AWS - Overview
date: 2019-01-28 15:50:23
tags:
- TOGAF
- Enterprise Architecture
---

>
http://pubs.opengroup.org/architecture/togaf9-doc/arch/


# TOGAF terminology

* __TOGAF__: Architecture Framework for Enterprise Architecture
* __framework__ = tools+method+common definitions
* __Architecture framework__: how we do architecture
* __ADM__ : Architecture Development Method
* __Architecture__: (ISO/IEC/IEEE 42010:2011) : The fundamental concepts or properties of a system in its environmental embodied in its elements, relationships, and in the principle of its design and evolution.
* __Architecture__ (TOGAF 9.2) : The structure of __components, their inner-relationships__, and the principles and guidelines __governing their design and evolution__ over time.

## Part 1: Intro

* Enterprise = High Level of Orgnization(s)
* EA domains - BDAT   
  * Business domain -- everthing aligns with Business Domain
  * Data domain
  * Application domain
  * Technology domain -- serve all the upper layers

* ADM : a process to define the enterprise architecture ; tested and repeatable
  * Preliminary stage: 准备阶段
  * Phase A ： Vison
  * Phase BCDE： BDAT
  * Phase F： migration plan
  * Phase G： 实施和管理
  * Phase H： 变更管理 -- 循环
  * 需求管理

* Deliverables, artifacts and building blocks
  * Artifacts = lists+matrices+diagrams
    * different ADM phase need different Artifacts
  * Deliverables: reviewed and signed off  
  * Building blockes:
     * ABB :
        * For Architecturual continuum
        * Sample: capability to look up a customer's info
     * SBB : Implemenation of ABB
        * Used for Solution conntinuum
        * sample: CRM customer search module

* Enterprise Continuum : a way to classifying item in architecture repo
    * Foundation Architecture
    * Common System Architecture  (like SOA)
    * Industry Architecture
    * Organization Architecture

* Architecture Repository
    * A Well Defined File Server / Git  / Knowledge management tool
    * The repo contains (9):
        * Archi metamodel
        * Archi Capability
        * Archi Landscape
        * Standard Information Base
        * Reference Library
        * Governance Log
        * Archi Requirement repository
        * Solutions Landsacpe

* Archi Capability
    * Evaluation in ADM preliminary phase
