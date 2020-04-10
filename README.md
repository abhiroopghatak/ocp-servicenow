# ocp-servicenow
ocp-servicenow

Providing Service Now an acessto create view priojects in OPENSHIFT 4 using service account

Prereqs:

  1. Openshift cluster platform 4 up and running 
  2. You have access to create cluster role , binding and service account .
  3. Basic understanding of user acess in Openshift objects
  4. Basic understanding of openshift4 api
  
  
  Steps : 
  1.  Create a project of any convention name .In my case I have created project with name 'servicenow'
  2.  create cluster role using the file 
      oc apply -f servicenow-cluster-role.yml
  3.  Create a serviceaccount 
      oc create sa  svcsnow
  4.  Create cluster role binding 
      oc apply -f  snow-cluster-role-binding.yml
