# Microservice Hands-On
- [Post-server](https://github.com/KyleKim107/post-server)
    - The Post-Server is responsible for managing social media posts in a microservice-based SNS system. It provides a REST API that allows users to create, retrieve, and delete posts.
        - API supports backward compatibility through versioning with an alternative endpoint:
            * `(GET /api/posts/user/{userId})`
        - Returns a list of SocialPostV2 objects.(Updated APIs)
            * `(GET /api/posts/v2/user/{userId})`

- [Image-server](https://github.com/KyleKim107/image-server)
    - The Image-Server is responsible for handling image uploads and retrievals in the SNS system. It provides a REST API for users to upload and view images.
        - Upload an Image
            - POST /api/images/upload
            - Accepts an image file (MultipartFile), stores it, and returns an image ID.
        - Retrieve an Image
            - GET /api/images/view/{imageId}

- [Timeline-server](https://www.google.com)
    The **Timeline-Server** manages user feeds in the SNS system, providing APIs to retrieve posts, manage likes, and generate personalized timelines.
        ### 1. **Retrieve All Feeds**
        - **GET** `/api/timeline`
        - Returns a list of all posts in the system.

    ### 2. **Retrieve a User's Feed**
        - **GET** `/api/timeline/{userId}`
        - Returns posts specific to a user.
- [User-server](https://github.com/KyleKim107/user-server)
- [Middleware](https://github.com/KyleKim107/middleware)
    - Provides a simple setup for middleware infrastructure using Docker. It includes configurations for:

        - MySQL: A relational database with a pre-configured schema (ddl.sql) and root password setup.
Kafka: A message queue system for distributed streaming, running on port 9092.
Redis: An in-memory data store for caching or fast data access, running on port 6379.
- [sns-frontend](https://github.com/KyleKim107/sns-frontend)
    - This repository provides the frontend UI for a microservice-based SNS (Social Networking Service) similar to Instagram or Facebook. Users can log in, upload photos, and view their feed.
        - Features

            - User Authentication: Log in functionality for users.

            - Photo Upload: Users can upload photos to their profile.

            - Feed Display: Users can view all the photos they have posted.




## Backward Compatibility
- When an API is changed, the old request and response methods must still be supported.
- For example, if a new property is added to a request, it should not be required, and the request should still return the same response even if the new property is left empty.
- When the response changes, the existing response must be maintained, and only new responses can be added, without modifying the key-value structure of the original response.
- In strong-typed languages like Java, controlling this is relatively easy, but in Kotlin, issues like nullable settings can cause compatibility problems and errors.
- In languages like TypeScript or Kotlin, adding something extra to the response may cause errors, so it’s important to configure the system to ignore unknown properties during deserialization.

## API Versioning
- If it’s not possible to maintain the existing request and response formats, a new version of the API should be created.
- API versions are usually separated by paths like `/api/v1`, `/api/v2`.
- When creating a new version of the API, the old version must still be provided, and care must be taken to ensure that changes to the new version do not affect the old version.
- Managing two versions of an API increases overhead, and as versions grow, it becomes more difficult to maintain and prone to mistakes.
- If the old version of the API is no longer in use, it’s best to delete it. The usage status can be checked either through communication within the team or by monitoring to ensure there are no further calls to the endpoint.
- A common practice is to gradually phase out the old version after deploying the new one.
- This approach applies not only to HTTP but also to message queues. By adding versioning to message properties or headers, you can ensure messages are consumed according to the correct version.

![API Versioning](image/api-versioning.png)