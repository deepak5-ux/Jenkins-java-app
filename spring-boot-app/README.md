# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml 
at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

## Execute the application locally and access it using your browser

Checkout the repo and move to the directory

```
git clone https://github.com/deepak5-ux/Jenkins-java-app.git
cd sprint-boot-app
```

Execute the Maven targets to generate the artifacts

```
mvn clean package
```

The above maven target stroes the artifacts to the `target` directory. You can either execute the artifact on your local machine
(or) run it as a Docker container.

** Note: To avoid issues with local setup, Java versions and other dependencies, I would recommend the docker way. **


### Execute locally (Java 11 needed) and access the application on http://localhost:8080

```
java -jar target/spring-boot-web.jar
```

### The Docker way

Build the Docker Image

```
docker build -t java-cicd-pipeline:v1 .
```

```
docker run -d -p 8010:8080 -t java-cicd-pipeline:v1
```

Hurray !! Access the application on `http://<ip-address>:8010`

### 📌 Next Steps

Now that you have the application running locally, try extending this project into a full CI/CD pipeline using the following tools:

**Jenkins** – Automate build, test, and deployment

**Docker** – Build and publish container images

**Kubernetes (Minikube/Cluster)** – Deploy and manage application at scale

**Argo CD** – GitOps-based continuous delivery for Kubernetes

👉 A complete guide for this end-to-end pipeline is provided in the root-level README of this repository.
```

Hurray !! Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 


