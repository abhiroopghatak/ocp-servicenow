# ocp-servicenow

Providing third party apps e.g. Service Now, an acess to create view projects , quotas etc in OPENSHIFT 4 using service account.

Prereqs:

  1. Openshift cluster platform 4 up and running 
  2. Cluster Admin access to create cluster role , binding and service account etc.
  3. Basic understanding of user RBAC in Openshift objects
  4. Basic understanding of openshift4 api
  
  
  There are two ways to automate the project creation using apis in Openshift.
    a. api driven : Using different apis for each individual resources to create , update , delete etc operation.
        api driven project creation files are placed in /apidriven directory.
    b. template driven : Create one project request template and use api to instantiate template passing parameters.
        template drivenproject creation files are placed in /template-driven directory.
  
  <b>a. api-driven project automation in Openshift</b>
  
    Steps : 
      1.  Create a project of any convention name .In my case I have created project with name 'servicenow'
              $oc new-project servicenow
      2.  Create cluster role .
              $oc apply -f servicenow-cluster-role.yml
      3.  Create a serviceaccount.
              $oc create sa  my-service-account
      4.  Create cluster role binding.
              $oc apply -f  snow-cluster-role-binding.yml
      5.  Prepare request json bodies of each object that we planning to create .
          Here we are creating project , resource quotas , limit ranges, rolebindings etc.
          Check sample json in requests files e.g.
          https://raw.githubusercontent.com/abhiroopghatak/ocp-servicenow/master/apidriven/ProjectCreation.yml

      6. Api execution flow :
          projectrequests =>update namespace => resourcequotas creation => limitranges creation 
          =>rolebindings creation to provide user group the access => delete admin role binding of this 
          serviceaccount to revoke any further access to this account.
          
  <b>b.  Openshift template driven project automation in Openshift<b>
  
  Steps : 
  
      1.  Create a namespace e.g. 'myprojectautomation' in openshift.
              $oc new-project myprojectautomation
      2.  Create a serviceaccount 'my-service-account'
              $oc create sa  my-service-account
      3.  Create a cluster role where you put resources , api grpoups and verbs .
             $oc apply -f template-driven/sa-role.yaml
      4.  Craete a role binding and provide the above role to your serviceaccount. (verify namespaces /sa name in files)
             $oc apply -f  snow-cluster-role-binding.yml
      5.  Access service account 'template-instance-controller' in project openshift-infra .
             $oc describe sa template-instance-controller -n openshift-infra
      6.  Create a role binding and provide same role (step 3) to this service account.
             $oc apply -f /template-driven/template-instance-controller-role-binding.yml
      7.  Create a project request template in your namespace 'myprojectautomation' 
          mentioning all parameters that are dynamic.
      8.  Create a secret in the same namespace mentioning all param values.
          #Curl request provided at : /template-driven/templateinstantiation.txt
      9.  Post api call to (#Curl request provided at : /template-driven/templateinstantiation.txt)
          https://$ENDPOINT/apis/template.openshift.io/v1/namespaces/$NAMESPACE/templateinstances
          providing the secret , you just craeted.
      10. Verify  template instance object has been craeted or not.
      11.  Access template instance object $oc describe template $template-instance-object-name -o yaml
      12.  Verify and confirm the status is Success.

  
