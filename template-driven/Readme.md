### This project is to Design and develop automated projects while accessing Openshift via Rest Api and instantiate a template ###


Steps : 
  1.  Create a namespace e.g. 'myprojectautomation' in openshift.
  2.  Create a serviceaccount 'my-service-account'
  3.  Create a cluster role where you put resources , api grpoups and verbs .
  4.  Craete a role binding and provide the above role to your serviceaccount.
  5.  Access service account 'template-instance-controller' in project openshift-infra .
  6.  Create a role binding and provide same role (step 3) to this service account.
  7.  Create a project request template in your namespace 'myprojectautomation' mentioning all parameters that are dynamic.
  8.  Create a secret in the same namespace mentioning all param values.
  9.  Post api call to https://$ENDPOINT/apis/template.openshift.io/v1/namespaces/$NAMESPACE/templateinstances provide your secret.
  10. Verify  template instance object has been craeted or not.
  11.  Access template instance object $oc describe template $template-instance-object-name -o yaml
  12.  



