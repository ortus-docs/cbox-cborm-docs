#Virtual Entity Service
The virtual entity service is another support class that can help you create virtual service layers that are bounded to a specific ORM entity for convenience. This class inherits from our Base ORM Service and allows you to do everything the base class provides, except you do not need to specify to which entityName you are working with. You can also use this class as a base class and template out its methods to more concrete usages. The idea behind this virtual entity service layer is to allow you to have a very nice abstraction to all the CF ORM capabilities (hibernate) and promote best practices.

![](https://github.com/ColdBox/cbox-cborm/wiki/VirtualEntityService.jpg)

