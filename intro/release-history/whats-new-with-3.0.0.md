---
description: February 12, 2021
---

# What's New With 3.0.0

## Compatibility Updates

In this major release we have two issues that are not backward compatible:

### `asQuery` now false

All properties and arguments that received the `asQuery` argument are now defaulted to false. Meaning arrays of objects/structs is now the default instead of query objects.  If you want to go back to queries, then make sure you add the `asQuery : true` to the method calls.

### cbValidation => cbi18n v2 Upgrade

Our supporting modules have been also upgraded to their major versions mostly to support cbi18n v2. [https://coldbox-i18n.ortusbooks.com/intro/release-history/whats-new-with-2.0.0](https://coldbox-i18n.ortusbooks.com/intro/release-history/whats-new-with-2.0.0)

If you are using localization features with cborm then you must read the compat guide for cbi18n v2.

## Release Notes

### Bugs

* \[[CBORM-2](https://ortussolutions.atlassian.net/browse/CBORM-2)] - `isDirty()` not working with `ActiveEntity` due to missing entity passed

### New Features

* \[[CBORM-3](https://ortussolutions.atlassian.net/browse/CBORM-3)] - Updated `cbValidation` to v3 to support cbi18n v2
* \[[CBORM-4](https://ortussolutions.atlassian.net/browse/CBORM-4)] - `asQuery` update to default it to false
* \[[CBORM-5](https://ortussolutions.atlassian.net/browse/CBORM-5)] - Document v3 variant in the docs

### Improvements

* \[[CBORM-6](https://ortussolutions.atlassian.net/browse/CBORM-6)] - Source code cleanups by applying formatting rules
