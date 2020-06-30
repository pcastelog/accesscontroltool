[![Build Status](https://travis-ci.org/Netcentric/accesscontroltool.svg?branch=develop)](https://travis-ci.org/Netcentric/accesscontroltool) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/biz.netcentric.cq.tools.accesscontroltool/accesscontroltool/badge.svg)](https://maven-badges.herokuapp.com/maven-central/biz.netcentric.cq.tools.accesscontroltool/accesscontroltool) [![License](https://img.shields.io/badge/License-EPL%201.0-red.svg)](https://opensource.org/licenses/EPL-1.0)

Access Control Tool for Adobe Experience Manager
================================================

The Access Control Tool for Adobe Experience Manager (AC Tool) simplifies the specification and deployment of complex [Access Control Lists in AEM](http://docs.adobe.com/docs/en/cq/current/administering/security.html#Access%20Control%20Lists%20and%20how%20they%20are%20evaluated).
Instead of existing solutions that build e.g. a content package with actual ACL nodes you can write simple configuration files and deploy them with your content packages. See [Comparison to other approches](docs/Comparison.md) for a comprehensive overview.

Features:
* easy-to-read Yaml configuration file format
* run mode support
* automatic installation with install hook
* cleans obsolete ACL entries when configuration is changed
* ACLs can be exported
* stores history of changes
* ensured order of ACLs
* built-in expression language to reduce rule duplication

See also our talk at [AdaptTo 2016](https://adapt.to/2016/en/schedule/ac-tool.html)

# Requirements

The AC Tool requires **Java 7 and AEM 6.1 (SP1)** or above (use v1.x for older versions) for on-premise installations. Since v2.5.0 **[AEM as a Cloud Service](https://www.adobe.com/marketing/experience-manager/cloud-service.html)** is supported, see [Startup Hook](https://github.com/Netcentric/accesscontroltool/blob/develop/docs/ApplyConfig.md#startup-hook) for details.

It is also possible to run the AC Tool on **Apache Sling 11** or above (ensure system user `actool-service` has `jcr:all` permissions on root). When using the AC Tool with Sling, actions in ACE definitions and encrypted passwords cannot be used. To use the `externalId` attribute, ensure bundle `oak-auth-external` installed (not part of default Sling distribution).

# Installation

The package is available from [the Maven Central repository](https://repo1.maven.org/maven2/biz/netcentric/cq/tools/accesscontroltool/accesscontroltool-package/). Install it e.g. via CRX package manager.

```
    <groupId>biz.netcentric.cq.tools.accesscontroltool</groupId>
    <artifactId>accesscontroltool-package</artifactId>
```

## Oak Index for rep:ACL

To retrieve all ACLs in the system, an oak index for node type `rep:ACL` is 

* required for versions < 2.4.0 (otherwise the performance degrades significantly)
* beneficial for large installations for versions >= 2.4.0 (see [#386](https://github.com/Netcentric/accesscontroltool/issues/386), most installations will be fine without index)

You can get the ZIP file via [Maven](https://repo1.maven.org/maven2/biz/netcentric/cq/tools/accesscontroltool/accesscontroltool-oakindex-package/). Install it e.g. via CRX package manager.

```
    <groupId>biz.netcentric.cq.tools.accesscontroltool</groupId>
    <artifactId>accesscontroltool-oakindex-package</artifactId>
```

# Migration to AC Tool

You can easily migrate to AC Tool following [four simple steps](docs/Migration.md).

# Configuration of ACL entries

You need to setup [Yaml configuration files](docs/Configuration.md) to specify your users, groups and ACL entries. See also the [best practices](docs/BestPractices.md) for hints on structuring.

There are also some [advanced configuration options](docs/AdvancedFeatures.md) supported such as loops, conditional statements and permissions for anonymous.

# Applying the ACL entries

There are multiple options to [apply the ACL entries](docs/ApplyConfig.md) (e.g. install hook, JMX and upload listener) to your target system.

# JMX interface

The [JMX interface](docs/Jmx.md) provides utility functions such as installing and dumping ACLs or showing the history. 

# History service

A history object collects messages, warnings, and also an exception in case something goes wrong. This history gets saved in CRX under /var/statistics/achistory. The number of histories to be saved can be configured in the history service.

# Building the packages from source

If needed you can [build the AC Tool yourself](docs/BuildPackage.md).

# License

The AC Tool is licensed under the [Eclipse Public License - v 1.0](LICENSE.txt).
