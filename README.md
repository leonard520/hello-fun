# hello-fun

This repo provides a simple Hello web app based on Spring Boot and Spring Cloud Function.

It can be deployed as a standalone web app, as a Kubernetes Deployment and Service, or as a Knative Service.

## The code

> **NOTE**: The project is configured for Java 11, if you are using Java 8, then modify the `java.version` property in `pom.xml`.

The project contains the following code layout:

```text
.
├── README.md
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── example
    │   │           └── helloapp
    │   │               └── HelloAppApplication.java
    │   └── resources
    │       └── application.properties
    └── test
        └── java
            └── com
                └── example
                    └── helloapp
                        └── HelloAppApplicationTests.java
```

It also contains some deployment manifests, depending on the deploymentType selected when generating the project.

You can modify the source code using [Visual Studio Code](https://code.visualstudio.com/):

```bash
code .
```

The Function that is used by this app is located at `src/main/java/com/example/helloapp/HelloAppApplication.java`

You can build the project using the provided Maven wrapper:

```bash
./mvnw clean package
```

## Standalone app with embedded Tomcat server

To run the app using the embedded Tomcat server you can run this command:

```bash
./mvnw spring-boot:run
```

You can access the function using `curl`:

```bash
curl -w'\n' -H 'Content-Type: text/plain' localhost:8080 -d "Fun"
```
## Deploying to Kubernetes using Kubernetes Deployment and Service

<img src="https://kubernetes.io/images/kubernetes-horizontal-color.png"
     alt="Kubernetes" width="400" />

You can containerize this template app and deploy it as a Deployment and Service on Kubernetes.
See the [Spring Boot Kubernetes](https://spring.io/guides/gs/spring-boot-kubernetes/) Guide for details.

We have included both `skaffold.yaml` and `Tiltfile` files to make this easier when deploying to a local cluster like [minikube](https://minikube.sigs.k8s.io/) or [kind](https://kind.sigs.k8s.io/).

You can install Skaffold by following these instructions: https://skaffold.dev/docs/install/

### Deploying to local cluster using Tilt

You can install Tilt by following these instructions: https://docs.tilt.dev/install.html

You also need to have `pack` from Cloud Native Buildpacks installed, see: https://buildpacks.io/docs/tools/pack/

To build and deploy the app run:

```
tilt up
```

and follow the instructions (hitting space bar brings up the Tilt interface in your browser).

To uninstall the app run:

```
tilt down
```

### Deploying to a cluster using Skaffold

To build and deploy the app to a local cluster run:

```
skaffold run -p local --default-repo dev.local --port-forward
```

To build and deploy the app to a remote cluster run:

```
skaffold run --default-repo <your-docker-id> --port-forward
```

To uninstall the app run:

```
skaffold delete
```

### Deploying as a native image to a cluster using Skaffold

> **NOTE:** The native image compilation is resource intensive and takes several minutes.

See the [System Requirements](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle/#getting-started-buildpacks-system-requirements) note in the Spring Native documentation for suggestions on Docker configuration.

To build and deploy the app as a native image to a local cluster run:

```
skaffold run -p local -p native --default-repo dev.local --port-forward
```

To build and deploy the app as a native image to a remote cluster run:

```
skaffold run -p native --default-repo <your-docker-id> --port-forward
```

To uninstall the app run:

```
skaffold delete
```

### Accessing the app deployed to your cluster

If you don't have `curl` installed it can be installed using downloads here: https://curl.se/download.html

To invoke the deployed function run the following `curl` command in another terminal window:

```
curl localhost:8080 -w'\n' -H 'Content-Type: text/plain' -d Fun
```
