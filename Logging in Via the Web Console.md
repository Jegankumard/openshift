## Discovering the web console URL

```
oc get pods -n openshift-console | grep console
> console-7d599cbf78-4xc9v     1/1     Running   0          23mD

oc get routes console -n openshift-console -o jsonpath='{"https://"}{.spec.host}{"\n"}'
> https://console-openshift-console.crc-lgph7-master-0.crc.q82njnglzds2.instruqt.io
```
You'll be presented with the login page in the web console as shown in the figure below:

Login
```
Username: developer
Password: developer
```
Upon successful login, you are presented with a "Getting Started" message and the option of creating a new project.

Click the create a Project link as shown in the figure below.
Create Project
Name project

To see the details of the project you just created, click the Project tab on the vertical menu bar on the left of the Web Console, as shown in the figure below.
Select Project
