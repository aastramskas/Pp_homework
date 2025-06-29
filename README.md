# Podbase Application overview

In it's current state I highly disagree that this application is ready to be deployed to the final user. All three modules has a lot of issues.

1. All GET endpoints have issues with data filtering, there is no way to filter information depending on parameters (status, dealerId, manufacturerId, model). These problems will be a headache for end user when trying to view information in frontend application. Also there is no pagination functionality, looking to the future it should be implemented, as without it, when database is populated fully GET requests will take a lot of resources, response payload will increase as well as response time. In the meantime, until the pagination functionality is implemented, I recommend adding endpoint caching.

3. When talking about POST endpoints, few major issues arises. The biggest one is that you can create new entities, even when API response was an error message. This allows users to populate database with useless and incorrect data, and it should be strictly avoided. Few good things are that we have required parameters, which in theory should reduce bad requests, but even with bad requests we allow data to be created. I have not found any duplication protection in POST /cars and /dealers endpoints. How can the end user find the correct car model or dealer name if there are 20 entities with the same data? 
