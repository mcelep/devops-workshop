# Devops-Workshop

# Challenge
## Day 1

Build & deploy a simple application based on one of the three runtimes: Spring Boot, Thorntail(RH implementation of Eclipse Microprofile, formerly known as Wildfly Swarm), vert.x

[Here](https://github.com/mcelep/spring-boot-hello-world) is a simple app based on Spring Boot, you are however free to use anything you like.

#### 1st phase

Goal is to have your application deployable via an [Openshift template](https://docs.openshift.com/container-platform/3.10/dev_guide/templates.html)

- Application needs to be configurable via a Configuration Map or a Secret
- Don't use ```oc new-app``` command, create a BuildConfig(```oc new-build```) , Service (```oc create service```), DeploymentConfig and a route (```oc expose ```) by using the oc cli tool.
- Use the Java - Source-to-Image (S2I) strategy with your BuildConfig
- Create a template for your application, there should be at least one configurable parameter in your template. 
(tip: oc export is your friend) Moreover, all the objects that are created via this template should have a common label e.g. app= myapp
- DeploymentConfig and BuildConfig both should have [resource limits](https://docs.openshift.com/container-platform/3.11/dev_guide/deployments/basic_deployment_operations.html#deployment-resources) set to 512M memory and 0.5 CPU
- Set an extra tag for the image you build using ``` oc tag v0.1.2.3``` e.g. a tag which denotes version from pom.xml. 
 
#### 2nd phase

Product owner of application is very keen on introducing a new feature and the code you introduce for this change is very risky.
You decided to introduce this change in a separate branch. New code will need to be tested end-to-end using the same project you used in the previous phase.

- Application(with all components) needs to be deployable in the same namespace based on a template that takes in parameters. Based on template parameters, all object names, labels(and whatever else is required to deploy app several times within the same namespace) needs to be configurable.  
- Check in your template to the source code repository.


#### 3rd phase

Product owner, wants to make sure that the user experience for this application is as smooth as possible and he wants to have zero-downtime during deployments.
You've done your research and found that [Blue/Green](https://martinfowler.com/bliki/BlueGreenDeployment.html) might be a good way to go about it.
So you will introduce a blue/green deployment for your application.

- Jenkins would be great for such a task but first make sure that you can create a blue/green deployment also using a bash script.
- Deploy application two times with your template and make sure that objects/resources have proper names e.g. a deploymentconfig with name *hello-world-blue* and *hello-world-green*  
- Single route as entry point for the application 
- During deployment time, decide which service(e.g. blue) is currently active and set the route to the non-active service(e.g green).
- Are you sure that your DeploymentConfigs has the right triggers set on them?
- [Here](https://github.com/mcelep/ocp-adv-app-dev/blob/master/dev/Jenkinsfile) is an example pipeline that does blue/green deployment

## Day 2

#### 4th phase

Your application has been a great success, and you get notifications about certain events. However, you want the system to react to load automatically.
Maybe it's time to start testing out [Horizontal Pod Autoscaling/HPA](https://docs.openshift.com/container-platform/3.11/dev_guide/pod_autoscaling.html).

#### 5th phase

Some users of application complain that application is not reachable every once in a while.
You want to analyse how often pods get restarted. You do some research and see that [Prometheus](https://prometheus.io/) is very popular in Kubernetes world , so you decide to give it a try.
Cluster you run your application does not have an Prometheus operator available. Will that stop you from installing Prometheus? 

- First you want to check your application logs. If your application does not log anything, make sure it does. After that go to Kibana which is GUI for seeing(on a pod, go to tab *Logs* and search for a link called *View Archive*).
For more details about the logging format read [this blog post](https://developers.redhat.com/blog/2018/01/22/openshift-structured-application-logs/).) 
- Deploy prometheus based on a trusted open source template
- Configure alerts based on a condition which counts the number of restarts within a given period



## General Tips:

- To see the structure/schema of an OpenShift object, use oc explain, e.g. ```oc explain --recursive deploymentconfig```
- *Oc get* command(and many other oc commands) can apply filters based on labels, ``` oc get pods -l app=myapp ```
- Windows users which need bash, can use git bash(based on cygwin).
 


