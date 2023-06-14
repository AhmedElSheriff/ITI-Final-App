# About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Deployment Process

This Laravel application was created to be deployed on _AWS EKS cluster_ by an automated _Jenkins_ CI/CD pipeline

* First, a Dockerfile was created to automate the building process of the application and its dependencies 
* Then, Kubernetes deployment and service files were created to be able to deploy the application on a Kubernetes cluster and expose it to the public using service type LoadBalancer
* Finally, a Jenkinsfile was created to build the CI/CD pipeline that consists of two stages, the first one is responsible for building the dockerfile and pushing the image to DockerHub, the second stage is responsible for deploying the helm chart to the AWS EKS cluster