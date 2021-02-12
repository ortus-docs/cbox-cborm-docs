# Results

Once you have concatenated criterias together, you can execute the query via the execution methods. Please remember that these methods return the results, so they must be executed last.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Method</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>count( propertyName=&quot;&quot; )</code>
      </td>
      <td style="text-align:left">Note, count() can&apos;t be called on a criteria after list() has been
        executed.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>get()</code>
      </td>
      <td style="text-align:left">Retrieve one result only.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getOrFail()</code>
      </td>
      <td style="text-align:left">Convenience method to return a single instance that matches the built
        up criterias query, or throws an exception if the query returns no results</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>list(</code>
        </p>
        <p><code>max,</code>
        </p>
        <p><code>offset,</code>
        </p>
        <p><code>timeout,</code>
        </p>
        <p><code>sortOrder,</code>
        </p>
        <p><code>ignoreCase, asQuery=false</code>
        </p>
        <p><code>asStream=false</code>
        </p>
        <p><code>)</code>
        </p>
      </td>
      <td style="text-align:left">Execute the criterias and give you the results.</td>
    </tr>
  </tbody>
</table>

```javascript
// Get
var results = c.idEq( 4 ).get();

// Listing
var results = c
     .like("name", "lui%")
     .list();

// Count via projections of all users that start with the letter 'L' or 'l' using case-insensitive-like
var count = c.ilike("name","L%").count();
```

{% hint style="success" %}
**Tip:** You can call `count()` and `list()` on the same criteria, but due to the internal workings of Hibernate, you must call `count()` first, then `list()`.
{% endhint %}

