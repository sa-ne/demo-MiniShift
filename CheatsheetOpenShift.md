## OpenShift/oc cmd Cheatsheet
  $ oc version
  $ oc login -u system:admin
  $ oc get pods -n default (-w to follow progress)
  $ oc whoami
  $ oc new-project <project_name>
  $ oc status
  $ oc delete project <project_name>
  $ oc logout
  $ oc rsh
  $ oc new-app
  $ oc get svc
  $ oc describe pod|svc|dc|bc
  $ oc logs -f bc/hello

https://docs.openshift.org/latest/cli_reference/admin_cli_operations.html

https://docs.openshift.org/latest/cli_reference/basic_cli_operations.html
