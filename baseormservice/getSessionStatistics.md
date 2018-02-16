#getSessionStatistics
Information about the first-level (session) cache for the current session

###Returns

* This function returns *struct*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| datasource | string | false | --- | The default or specific datasource use |

###Examples

```javascript
// Let's get the session statistics
stats = ormService.getSessionStatistics;

// Lets output it
<cfoutput>
collection count: #stats.collectionCount# <br/>
collection keys: #stats.collectionKeys# <br/>
entity count: #stats.entityCount# <br/>
entity keys: #stats.entityKeys#
</cfoutput>
```

