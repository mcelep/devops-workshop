# devops-workshop

# Challenge
Build & deploy a simple application based on one of the three runtimes: Spring Boot, Thorntail(RH implementation of Eclipse Microprofile, formerly known as Wildfly Swarm, vert.x

[Here](https://github.com/mcelep/spring-boot-hello-world) is a simple app based on Spring Boot, you are however free to use anything you like.

#### 1st phase:

Goal is to have your application deployable via an [Openshift template](https://docs.openshift.com/container-platform/3.10/dev_guide/templates.html)

- Application needs to be configurable via a Configuration Map or a Secret
- Don't use ```oc new-app``` command, create a BuildConfig(```oc new-build```) , Service (```oc create service```), DeploymentConfig and a route (```oc expose ```) by using the oc cli tool.
- Use the Java - Source-to-Image (S2I) strategy with your BuildConfig
- Create a template for your application, there should be at least one configurable parameter in your template. 
(tip: oc export is your friend) Moreover, all the objects that are created via this template should have a common label e.g. app= myapp


#### 2nd phase:

Application Production Owner is very keen on introducing a new feature and the code you introduce for this change is very risky.
You decided to introduce this change in a separate branch. New code will need to be tested end-to-end using the same project you used in the previous phase.

- Application(with all components) needs to be deployable in the same namespace based on a template that takes in parameters. Based on template parameters, all object names, labels(and whatever else is required to deploy app several times within the same namespace) needs to be configurable.  
- Check in your template to the source code repository.


#### 3rd phase:



## General Tips:

- To see the structure/schema of an OpenShift object, use oc explain, e.g. ```oc explain --recursive deploymentconfig```
- Oc get command can filter based, ``` oc get pods -l app=myapp ```
 


