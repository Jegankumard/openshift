## Discovering the web console URL
Discovering the URL of the OpenShift web console is a two-step process. First, you need to confirm that the web console is indeed up and running in OpenShift. Then you need to query the OpenShift route resource object in order to get the URL for the web console running in the cluster.

Step 1: Run the following command to check that the pod responsible for the OpenShift web console is available. (You might have to wait a minute for the pod to be ready):
```
oc get pods -n openshift-console | grep console
```
You'll get output similar to the following:
> console-7d599cbf78-4xc9v     1/1     Running   0          23mD


Step 2: Run the following command to find the route to the OpenShift web console:
```
oc get routes console -n openshift-console -o jsonpath='{"https://"}{.spec.host}{"\n"}'
```
You'll get the URL to the web console that is special to your instance of OpenShift running in the Instruqt interactive learning environment. The following is an example URL. Yours will be different.
> https://console-openshift-console.crc-lgph7-master-0.crc.q82njnglzds2.instruqt.io

Step 3: Copy the URL displayed in the output from oc get routes and paste it into a window in your web browser.

You'll be presented with the login page in the web console as shown in the figure below:

Login

Username: developer
Password: developer
Upon successful login, you are presented with a "Getting Started" message and the option of creating a new project.

Step 4: Click the create a Project link as shown in the figure below.

Create Project

Step 5: Name the project myproject as shown in the figure below.

Name project

Step 6: To see the details of the project you just created, click the Project tab on the vertical menu bar on the left of the Web Console, as shown in the figure below.

Select Project
