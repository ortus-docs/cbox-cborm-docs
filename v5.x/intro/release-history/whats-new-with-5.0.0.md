---
description: What's new in cborm 5.0.0, the BoxLang Prime certified release that drops Lucee support
---

# What's New With 5.0.0

cborm 5.0.0 is a major release focused on the BoxLang runtime: the module is now certified for **BoxLang Prime** and drops the Lucee support that had been in maintenance mode. The API is unchanged from the cborm 4.x series: your services, virtual services, active entities, dynamic finders and criteria queries work the same.

{% hint style="warning" %}
Lucee is no longer supported. The cborm 4.x series keeps supporting Lucee 5/6 and Adobe ColdFusion 2023+. Read [Upgrading to 5](upgrading-to-5.md) before you upgrade.
{% endhint %}

## BoxLang Prime Certified

cborm is now certified and continuously tested against [BoxLang Prime](https://boxlang.io/prime), the commercially supported BoxLang runtime. The automated test matrix covers:

* BoxLang (open source)
* BoxLang BE
* BoxLang Prime

## Lucee Support Removed

Lucee ORM support, in maintenance mode since cborm 4, is removed. If your applications run on Lucee, stay on the cborm 4.x series.

## Supported Engines

* BoxLang 1.0+ (open source, BE and Prime)
* Adobe ColdFusion 2023+ (including Adobe 2025)
* Lucee: no longer supported

## Housekeeping

* All CI actions updated to their latest versions.
