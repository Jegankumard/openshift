## Goal
The goal of this track is to demonstrate how to copy files to and from a running container without rebuilding the container image. 
In addition, the track will demonstrate how to use a Persistent Volume and Persistent Volume claim under OpenShift to store data independent of a container.

## Concepts and techniques
Creating OpenShift projects and applications from the command line using the oc command
Copying files from a container and into a local machine using the oc rsync command
Using the implicit automation available in OpenShift to copy changed files on a local machine to a container's file system.
Work with a Persistent Volume and Persistent Volume claim independent of a container

## Use case
You use the concepts and techniques covered in this track when you want to manually or automatically synchronize changes in the files on a 
local machine with containers in an OpenShift cluster and vice versa.

### Creating an Initial Project
login
```
oc login -u admin -p admin https://api.crc.testing:6443 --insecure-skip-tls-verify=true
```
create a new project named myproject
```
oc new-project myproject
```
output
```
Now using project "myproject" on server "https://api.crc.testing:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname
```
### Downloading files from a container using the oc rsync command
You'll create an application from an existing container image hosted in the Quay.io container registry. 
Then you will access a command line shell within the application's underlying Linux container. 
Finally, you'll use the oc rsync command from the shell's command line to copy a file out of the container and into the local machine.


Step 1: Run the following command to create an OpenShift application named simplemessage 
based on the container image quay.io/openshiftlabs/simplemessage:1.0:
```
oc new-app quay.io/openshiftlabs/simplemessage:1.0 --name simplemessage
```
You'll get output similar to the following:
```
--> Found container image f5becdb (28 minutes old) from quay.io for "quay.io/openshiftlabs/simplemessage:1.0"

    Node.js 14
    ----------
    Node.js 14 available as container is a base platform for building and running various Node.js 14 applications and frameworks. Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

    Tags: builder, nodejs, nodejs14

    * An image stream tag will be created as "simplemessage:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "simplemessage" created
    deployment.apps "simplemessage" created
    service "simplemessage" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the 
	commands below:
     'oc expose service/simplemessage'
    Run 'oc status' to view your app.
```
Step 2: Run the following command to monitor the deployment of the application:
```
oc rollout status deployment/simplemessage
```
You'll get output similar to the following:
```
Waiting for deployment "simplemessage" rollout to finish: 0 of 1 updated replicas are available...
deployment "simplemessage" successfully rolled out
The command will exit once the deployment has completed and the web application is ready.
```
Step 3: Run the following command to see the name of the pods corresponding to the running containers for this application:
```
oc get pods --selector deployment=simplemessage
```
You only have one instance of the application so only one pod will be listed in the output, similar to the following:
```
NAME                             READY   STATUS    RESTARTS   AGE
simplemessage-7fcb66cfb6-hmdpd   1/1     Running   0          5m45s
```
A pod created in OpenShift will have a name that's based on information determined at runtime. 
You will need the actual name of the pod that's running the SimpleMessage application.

You'll use the pod name in subsequent steps, so let's start by getting the name of the pod and 
storing it in an environment variable named POD.

Step 4: Run the following command to extract the simplemessage pod name from the OpenShift cluster and store it in the environment variable named POD:
```
POD=$(oc get pods --selector deployment=simplemessage -o custom-columns=NAME:.metadata.name --no-headers); echo $POD
```
You'll get output similar to the following, but your output will differ because OpenShift creates the name 
at runtime based on information specific to the running instance of the cluster:
```
simplemessage-7fcb66cfb6-hmdpd
```
Step 5: Run the following command to create an OpenShift service resource that exposes the pod with the parksmap container to the internal OpenShift network. The command will also create a route resource that has a URL allowing public access to the application from the Internet.
```
oc expose service/simplemessage
```
You'll get output similar to the following.
```
route.route.openshift.io/simplemessage exposed
```
Step 6: Run the following command to get the URL to the SimpleMessage application that you'll access in a web browser:
```
export APP_ROUTE=`oc get route simplemessage -n myproject -o jsonpath='{"http://"}{.spec.host}'` && echo $APP_ROUTE
```
You'll get output similar to the following:
```
http://simplemessage-myproject.crc-lgph7-master-0.crc.ogrx0anbjp74.instruqt.io
```
Remember: The output shown above is special to this running instance of OpenShift. 
The URL you get when you run this track will be different.

Copy the URL returned in the output above and then copy it into a browser. 
The URL will take you to the SimpleMessage application's web page as shown in the figure below.

Original Web Sites

Step 6: Run the following oc rsh command to open an interactive command shell within the pod whose name is defined in the environment variable POD:
```
oc rsh $POD
```
You'll get output similar but not exactly like the following:
```
sh-4.2$
```
Step 7: Run the following command from within the interactive shell in order to see files in the application's directory:
```
ls -las
> total 32
 0 drwxrwxr-x.  1 default root   152 Mar 29 15:08 .
 0 drwxrwxr-x.  1 default root    17 Sep 15  2021 ..
 0 drwx------.  3 default root    25 Mar 29 15:08 .config
 0 drwxr-xr-x.  4 default root    70 Mar 29 15:08 .npm
 0 drwxr-xr-x.  2 default root     6 Mar 29 15:08 .npm-global
 0 drwxrwxr-x.  1 default root    19 Sep 15  2021 .pki
 4 -rw-r--r--.  1 root    root   998 Mar 29 15:01 index.js
 4 -rw-r--r--.  1 root    root    16 Mar 28 22:57 message.txt
 4 drwxr-xr-x. 52 default root  4096 Mar 29 15:08 node_modules
16 -rw-r--r--.  1 default root 14411 Mar 29 15:08 package-lock.json
 4 -rw-r--r--.  1 root    root   285 Mar 28 22:57 package.json
```
This is a Node.js application. Node.Js is an interpreted programming language. 
The source code is readily available for editing and immediate deployment, with no compilation required.

Step 8: Run the following command to confirm the directory within the container where the files are stored:
Step 9: Run the following to exit the interactive shell
```
pwd
> /opt/app-root/src
exit
```

Next you will use the oc rsync command to copy files from the container into the local machine.

The form of the command when copying a single file from the container to the local machine is:
```
oc rsync <pod-name>:/remote/dir/filename ./local/dir
```
Step 10: Run the following set of commands to navigate to the /opt directory, create a directory named message, and then navigate into the directory message.
```
cd /opt && mkdir message && cd message
```
Step 11: Run the following command to copy the message.txt file from the /opt/app-root/src/ directory in the application's container and into the /opt/message directory:
```
oc rsync $POD:/opt/app-root/src/message.txt .
```
check file
```
ls -las
total 4
0 drwxr-xr-x. 3 root root 38 Mar 29 03:55 .
0 drwxr-xr-x. 4 root root 75 Mar 29 03:55 ..
0 drwxr-x---. 3 root root 19 Mar 29 03:55 .kube
4 -rw-r--r--. 1 root root 16 Mar 28 22:57 message.txt
```
NOTE:
The local directory into which you want the file copied must exist prior to the copy activity. 
If you don't want to copy the message.txt into the current directory, ensure that the target 
directory you intend to use was created beforehand.
In addition to copying a single file, a directory can also be copied. The form of the command 
when copying a directory to the local machine is:



NOTE:
```
If the target directory contains existing files with the same name as a file in the container, 
the local file will be overwritten.

If there are additional files in the target directory that don't exist in the container, 
those files will be left as is.
```
> If you did want an exact copy, where the target directory was always updated to be 
exactly the same as what exists in the container, use the --delete option to oc rsync.
When copying a directory, you can be more selective about what is copied by using 
the --exclude and --include options to specify patterns to match against directories and files.

> If there is more than one container running within a pod, you will need to specify 
which container you want to work with by using the --container option.

###


###


###


###


###


###
