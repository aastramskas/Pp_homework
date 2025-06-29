# Podbase Application overview

In it's current state I highly disagree that this application is ready to be deployed to the final user. All three modules has a lot of issues.

1. All `GET` endpoints have issues with data filtering, there is no way to filter information depending on parameters (`status`, `dealerId`, `manufacturerId`, `model`). These problems will be a headache for end user when trying to view information in frontend application.

    Also there is no pagination functionality, looking to the future it should be implemented, as without it, when database is populated fully `GET` requests will take a lot of resources, response payload will increase as well as response time. In the meantime, until the pagination functionality is implemented, I recommend adding endpoint caching.

2. When talking about `POST` endpoints, few major issues arises. The biggest one is that you can create new entities, even when API response was an error message. This allows users to populate database with useless and incorrect data, and it should be strictly avoided. Few good things are that we have required parameters, which in theory should reduce bad requests, but even with bad requests we allow data to be created.

    I have not found any duplication protection in `POST` `/cars` and `/dealers` endpoints. How can the end user find the correct car model or dealer name if there are 20 entities with the same data?
    
    Not all `POST` parameters works as intended. Like for `/cars` `status` parameter does nothing. It can have two values `SUPPORTED` && `UNSUPPORTED`, but when you want to create a new car with status `UNSUPPORTED` it is always created with status `SUPPORTED`

3. Main problem with `PATCH` endpoints is that they do not perform their direct functions. Their main function is for updating existing data, but not all parameters can be changed. For example: `/dealers` endpoint does not update `manufacturerId` , `carIds`.

4. And then we have `DELETE` endpoints. This is the main reason why I think application can't be released. As per few cases, when trying to delete already deleted entity and trying to delete entity with bad `_id`, application can't handle the request and in response it kills the application. This is a major concern as any bad request disturbs workflow for all users. currently it can only be fixed with direct developer intervention.

5. Current API have no security implementation. All Methods can be called without authentification, and no endpoint is secured by authorization token. This means that any external party can exploit the `DELETE` methods and leave the application killed indefinitely.

AUTOMATION PLANS

  At the moment I do not recommend any automation, as the application is not stable enought for it. At the start all functions should be covered by unit tests, as it will be easier and cheaper to fix all the quirks. After that the best module for automation would be manufacturers as it has direct links to both `/cars` and `/dealers` endpoints. It will allow for basic integration automation between the modules. 
