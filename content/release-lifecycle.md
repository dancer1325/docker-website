---
title: Docker's product release lifecycle 
description: Describes the various stages of feature lifecycle from beta
  to GA.
keywords: beta, GA, Early Access,
---

* goal
  * Docker's product release lifecycle
  * Docker's stages
  * product retirement process
    * uses |
      * features
      * products 

## Lifecycle stage

| Lifecycle stage  | Customer availability | Support availability | Limitations | Retirement |
| --- | --- | ---- | ---| ---|
|[Experimental](#experimental)| Limited availability | Community support | may have <br/> &nbsp; limitations, <br/> &nbsp; bugs <br/> &nbsp;  stability concerns |⚠️ Can be discontinued without notice ⚠️|
|[Beta](#beta) | All or those involved in a Beta Feedback Program | Community support | may have <br/> &nbsp; limitations, <br/> &nbsp; bugs <br/> &nbsp;  stability concerns | ⚠️Can be discontinued without notice ⚠️|
| [Early Access (EA)](#early-access-ea) | All or those involved in an Early Access Feedback Program | Full | may have <br/> &nbsp; limitations(documented), <br/> &nbsp; bugs <br/> &nbsp;  stability concerns | Follows the [retirement process](#retirement-process) |
| [General Availability (GA)](#general-availability-ga) | All | Full | Few or NO limitations for supported use cases | Follows the [retirement process](#retirement-process) |

### Experimental

* == features / Docker is currently experimenting with. 
* Customers / access experimental features -> can
  * test,
  * validate,
  * provide feedback
* **Limitations:**
  * _Example:_
    * functional limitations,
    * performance limitations,
    * API limitations

### Beta

* Beta
  * == initial releases -- of -- potential future products OR features
* **Customer availability:**
  * by invitation (public or private)

### Early Access (EA)

* Early Access
  * == products or features /
    * may have potential feature limitations &
    * -- enabled for -- specific user groups
    * AFTER pending some fine tuning -> ready to be released to the world

### General Availability (GA)

* General Availability
  * == FULLY functional products OR features / openly accessible to ALL Docker customers

## Retirement process

* == decision to retire OR deprecate features
  * rigorous process / 
    * check the
      * demand,
      * use,
      * impact
      * customer feedback
    * exception
      * changes necessary -- to protect the -- integrity of our platform
        * -> accelerate timelines
  * guidelines
    - **Advance notice:** 
      - notify customers >= 6 months in advance
    - **Viable alternatives:**
      - offered from
        - Docker or
        - TP party providers
    - **Continued support:**
      - provide continued support for functionality | UNTIL its retirement date.
