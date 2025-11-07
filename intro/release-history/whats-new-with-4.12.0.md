---
description: What's new in CBOrm 4.12.0
---

# What's New With 4.12.0

CBOrm 4.12.0 is a minor release focused on BoxLang Prime compatibility, code quality improvements, and bug fixes.

## Added

### CI/CD Enhancements

* **Manual Workflow Dispatch**: Added `workflow_dispatch` trigger to cron.yml, allowing manual execution of scheduled workflows for testing and debugging

### BoxLang Runtime Compatibility

* **BoxLang-Only Runtime Support**: Added duplicate ORM settings specifically for BoxLang-only runtime environments where compatibility module aliases aren't available

### Code Quality

* **Full Var Scoping**: Implemented comprehensive var scoping throughout the codebase to eliminate IDE warnings and improve code clarity

## Fixed

### Critical Bug Fixes

* **[BOX-163](https://ortussolutions.atlassian.net/browse/BOX-163)**: Fixed critical dead code bug in `entityGivenName` method of `ORMUtilSupport.cfc` where the Hibernate result wasn't being returned, causing `null` errors

```javascript
// Before (broken):
try {
    var entityName = getSession(...).getEntityName(entity);
} catch (org.hibernate.TransientObjectException e) {
    // Falls through without returning entityName
}

// After (fixed):
try {
    return getSession(...).getEntityName(entity);
} catch (org.hibernate.TransientObjectException e) {
    // Falls through to metadata discovery
}
```

### BoxLang Prime Compatibility

* **Defensive Coding for BoxLang Prime**: Added extensive null checks and defensive programming patterns to ensure smooth operation with BoxLang Prime
* **ACF Isolation**: Moved CFIDE EventHandler interface reference to `ACFEventHandler` for better separation of Adobe-specific code
* **Interface Annotation Removal**: Dropped ACF-only interface annotations that caused compatibility issues with BoxLang Prime
* **Server.ColdFusion Blocks**: Added defensive coding around `server.coldfusion` checks for cross-engine compatibility

### Testing Infrastructure

* **BoxLang CFML Compatibility**: Updated to use `@be` tag for `bx-compat-cfml` in BoxLang server tests for better compatibility layer testing

{% hint style="warning" %}
If you were experiencing `null` errors when calling entity name operations, especially with transient entities, this release fixes that critical issue.
{% endhint %}

## Release Date

November 7, 2025
