# Google Cloud App Engine workshop
This is an introductory workshop to Google Cloud App Engine. In this repository you will find the nodejs/angular front-end application that the participants shall implement a back-end for. The front-end application has a configurable back-end so this application will only have to be deployed once, and then all the participants can use it. The intention is that the instructors has set this application up as the ```default``` service in a Google Cloud project that the workshop participants will be given access to.

The workshop will introduce the participants to
* Google Cloud SDK
* Google Cloud Console
* Basic Spring Boot
* Application deployment with Google Cloud

Instructors must speak about the following topics
* Spring Boot (basic introduction)
* Spring Rest services
    * ```@RestController```
    * ```@RequestMapping```
    * ```@CrossOrigin```
    * ```@RequestBody```
    * ```@PathVariable```
* CORS
* Google Cloud Console
    * App Engine
    * Logging

## Pre-requisites
* All participants must have a Google account
    * Can be Gmail or private email connected to a Google account
* Laptop
    * IntelliJ (recommended) or other Java IDE
    * JDK 8
    * Maven 3
    * nodejs (>= 6.10.1)
    * git
* Access to a Google Cloud Project with billing
    * Provided by instructors

## Setup
1. Install google-cloud-sdk:
    * https://cloud.google.com/sdk/docs
    * project: computas-universitet
    * Follow the quickstart guide for your O/S
2. Generate project
   1. Go to https://start.spring.io/
   2. Fill in the form and add 'web' as dependendy
   3. Click on generate and unzip somewhere on your computer
2. Launch IntelliJ
    1. Open the generated project 
3. Add the following to the ```<plugins>``` section of pom.xml
```
<plugin>
    <groupId>com.google.cloud.tools</groupId>
    <artifactId>appengine-maven-plugin</artifactId>
    <version>1.3.1</version>
</plugin>
```
4. Create file ```src/main/appengine/app.yaml``` replace ```<your service>``` with a unique identity for your backend
    implementation, e.g. ```ahr```
```
runtime: java
env: flex
service: <your service>

handlers:
- url: /.*
  script: this field is required, but ignored
  secure: always  # Require HTTPS

manual_scaling:
  instances: 1
```
6. Verify that your application works by running the ```*Application``` class (has a main-method)
6. You can now deploy the application ```mvn appengine:deploy```
7. Implement a REST service endpoint that has the following methods, see https://spring.io/guides/gs/rest-service/
    * ```GET /api/todos```
        * Returns the list of existing _Todo_-elements
    * ```POST /api/todos```
        * Accepts a _Todo_-element as RequestBody (payload)
        * Adds the _Todo_-element to the list of _Todo_-elements
        * Returns the list of existing _Todo_-elements (including the new)
    * ```DELETE /api/todos/{id}```
        * ```{id}``` is a PathVariable
        * Delete the _Todo_-element with the specified ```id```
        * Returns the list of existing _Todo_-elements (not-including the deleted)
    * All payloads (RequestBody and ResponseBody) shall be json
    * _Todo_ datastructure
    ```
    {
        "_id": "<some_unique_string_identifier>"
        "text": "<the todo text>"
    }
    ```

## Development
During development it is important to have a good process where the developer easily can test the implementation
without hassle. This is why developers always should have a local development environment where the application
can run so that one does not have to deploy to test it. With spring-boot you can launch your application as is on
your development computer, and we recommend that you also set up the front-end application to facilitate integration
testing.

### Start the back-end
Run the main-method of the ```*Application``` class.

### Front-end setup
1. Clone the node-todo repository ```https://github.com/mapster/node-todo.git```
2. Install dependencies. Run ```npm install``` in the cloned project
3. Start the front-end ```npm start```

You are now ready to start working.
