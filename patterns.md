# Code design patterns

## MVC

Model, view, controller.

Architectural design pattern, that consists in separating concerns of an application, in 3 distinct layers: 

#### Model: 

Concerned with defining the actual object that is being manipulated through the request/response. Example, a blog post service application, will handle a `post` object. This is the "real world" object that needs to be mapped. 

This `Model` layer, can be split into 2 more sub-layers, `Service` and `Repository`. 

`Service:` The business logic dependency, which will be injected (`Dependency Injection`) into the handler (controller). The service works on orchestrating and validating the data, preparing it to be sent in the handler.

`Repository:` Database operations concern, it will be injected into the `Service`. 


#### Dependency Injection 

Code pattern where objects receive their dependencies externally, instead of creating them internally. This allows objects to be more flexible about the implementations they use. 

For example, the handler (`Controller`) receives the `Service` from the exterior, depending only on its abstraction, instead of a concrete service. Besides making the application more scalable and flexible as a whole, it will also allow each module to be tested more easily, since we can inject mock services into them. 

### Controller

This is the layer responsible for receiving HTTP requests and returning them back through the HTTP response. This is where all the `handlers` will be written. The `Controller` depends on the `Service`, to get the logic job done, and only concerns itself with receiving and returning.

### View 




