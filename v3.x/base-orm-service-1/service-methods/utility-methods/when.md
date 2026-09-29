# when

This method gives you the ability to fluently create chains of executions by evaluating the incoming `target` as a `boolean`. If true it will execute the `success` closure, else the `failure` closure if passed.

## Returns

* The ORM Service so you can do concatenated calls

## Arguments

<table data-header-hidden><thead><tr><th width="150">Key</th><th width="150">Type</th><th width="150">Required</th><th width="150">Default</th><th>Description</th></tr></thead><tbody><tr><td>Key</td><td>Type</td><td>Required</td><td>Default</td><td>Description</td></tr><tr><td><code>target</code></td><td><code>boolean</code></td><td>Yes</td><td></td><td>A boolean evaluator</td></tr><tr><td><code>success</code></td><td><code>closure</code></td><td>Yes</td><td></td><td>The closure to execute if the target is true</td></tr><tr><td><code>failure</code></td><td><code>closure</code></td><td>No</td><td></td><td>The closure to execute if the target is false</td></tr></tbody></table>

## Examples

```javascript
baseService
.when( 
    !isNull( rc.createdDate ), 
    ( service ) => service.autoCast( "User", "createdDate", rc.createdDate )
)
.when(
    rc.search.len(),
    ( service ) => service.like( "name", rc.search ),
    ( service ) => service.isTrue( "search" )
)
```
