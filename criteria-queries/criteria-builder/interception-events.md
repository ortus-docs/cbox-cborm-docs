---
description: "The criteria interception points and how cborm relays them to ColdBox interceptors"
---

# Interception Events

The criteria builder announces interception points when conditions are added and when `list()`, `count()` and `get()` run. You can listen to them to log, audit or tweak criteria queries across your application.

| Interception point           | When | Data |
| ---------------------------- | ---- | ---- |
| `onCriteriaBuilderAddition`  | A condition is added | `criteriaBuilder`, `type` (the condition method) |
| `beforeCriteriaBuilderList`  | Before `list()` runs | `criteriaBuilder` |
| `afterCriteriaBuilderList`   | After `list()` runs | `criteriaBuilder`, `results` |
| `beforeCriteriaBuilderCount` | Before `count()` runs | `criteriaBuilder` |
| `afterCriteriaBuilderCount`  | After `count()` runs | `criteriaBuilder`, `count` |
| `beforeCriteriaBuilderGet`   | Before `get()` or `getOrFail()` runs | `criteriaBuilder` |
| `afterCriteriaBuilderGet`    | After `get()` or `getOrFail()` runs | `criteriaBuilder`, `result` |

The `type` of `onCriteriaBuilderAddition` is the condition's main name, so `eq()` is reported as `isEq`. A negated condition is reported with its full name, for example `notLike`.

## How cborm relays them

bx-orm announces these points on the BoxLang runtime. cborm registers them as ColdBox custom interception points and, when the module loads, registers a small bridge (`cborm.models.CriteriaEventBridge`) that re-announces each one to the ColdBox interceptor service. Your ColdBox interceptors listen to them as usual:

```javascript
// interceptors/CriteriaAudit.bx
class {

    property name="log" inject="logbox:logger:{this}";

    function onCriteriaBuilderAddition( event, data ){
        log.debug( "Criteria condition added: #data.type#" );
    }

    function afterCriteriaBuilderList( event, data ){
        log.debug( "Criteria list returned #data.results.len()# rows: #data.criteriaBuilder.getSQL()#" );
    }

}
```

```javascript
// config/ColdBox.bx
interceptors = [
    { class : "interceptors.CriteriaAudit" }
];
```

Because bx-orm announces the points for every criteria, they fire for criteria created with `newCriteria()` and for criteria created directly with `entityCriteria()`. Plain BoxLang interceptors registered on the runtime (`boxRegisterInterceptor()`) receive them too.

{% hint style="info" %}
**Changes from cborm 5**

* The interception point names and data keys are the same, but `criteriaBuilder` is now the bx-orm criteria builder.
* `onCriteriaBuilderAddition` fires for conditions only, and its `type` is the condition method (`isEq`, `like`, `notIn`, ...). In cborm 5 it was a category (`Restriction`, `Subquery`, `Offset`, `Max`, ...) and also fired for `firstResult()` and `maxResults()`.
* In cborm 5 the cborm criteria builder announced them directly to ColdBox; in cborm 6 bx-orm announces them and cborm's `CriteriaEventBridge` relays them. The bridge is removed when the cborm module unloads.
{% endhint %}
