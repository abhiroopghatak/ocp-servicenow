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
  2.  create cluster role using the yml file => 
      $oc apply -f servicenow-cluster-role.yml
  3.  Create a serviceaccount =>
      $oc create sa  my-service-account
  4.  Create cluster role binding =>
      oc apply -f  snow-cluster-role-binding.yml
