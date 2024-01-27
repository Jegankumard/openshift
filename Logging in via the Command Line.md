## Logging into OpenShift from the command line

Commands
```
oc login -u developer -p developer
oc new-project <projectname>
oc get projects
oc project <projectname>
oc whoami
oc whoami --show-server
> https://api.crc.testing:6443
```
In the case where an external authentication service is used as the identity provider, the login steps are a bit different.

When you log in at the command line using oc login without providing the username (-u) and password (-p) options, you will get an error similar to the following. 
The response directs you to an authentication server:

You must obtain an API token by visiting https://oauth-openshift.crc-lgph7-master-0.crc.d9avlfzludvk.instruqt.io/oauth/token/request
Once you get your API token, you receive a user's login credentials from the authentication server according to that server's authentication process.

Then, once you get the login information from the authentication server, return to the terminal window and execute oc login using those authentication credentials.
