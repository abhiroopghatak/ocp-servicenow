# ocp-servicenow
ocp-servicenow

Providing third party apps e.g. Service Now, an acess to create view projects , quotas etc in OPENSHIFT 4 using service account.

Prereqs:

  1. Openshift cluster platform 4 up and running 
  2. Cluster Admin access to create cluster role , binding and service account etc.
  3. Basic understanding of user RBAC in Openshift objects
  4. Basic understanding of openshift4 api
  
  
  There are two ways to automate the project creation using apis in Openshift.
    1. Using different apis for each individual resources to create , update , delete etc operation.
    2. Create one project request template and use api to instantiate template passing parameters.
    
  1. api driven project creation files are placed in /apidriven directory
  2. template drivenproject creation files are placed in /template-driven directory 
  
  Steps : 
  1.  Create a project of any convention name .In my case I have created project with name 'servicenow'
  2.  Create cluster role .
  3.  Create a serviceaccount.
  4.  Create cluster role binding.
  5.  Prepare request json bodies of each object that we planning to create .Here we are creating project , resource quotas , limit ranges, rolebindings etc.
  
      
      ###ROUGH###
      using the yml file => 
      $oc apply -f servicenow-cluster-role.yml
      
      $oc create sa  my-service-account
       =>
      oc apply -f  snow-cluster-role-binding.yml
      ###ROUGH###
      
  2.Openshift template driven project automation in Openshift
  
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
  12.  Verify and confirm the status is Success.

  
