The WireBox injection DSL has an injection namespace called entityService that can be used to wire in a Virtual Entity Service. You will use this DSL in conjunction with the name of the entity to manage.

| Inject Content | Description |
| --- | --- |
| entityService:{entity} | Inject a VirtualEntityService object for usage as a service layer based off the name of the entity passed in. |

```javascript


component{
    // Virtual service layer based on the User entity
    property name="userService" inject="entityService:User";
}
```

Once a virtual entity service is bounded it will be injected wherever you define it. Then just use it!