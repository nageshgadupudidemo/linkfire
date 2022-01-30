## Integration Kubernetes with Jenkins

### Pre-requisites

1. Jenkins server 
2. Ansible server
3. Kubernetes cluster
 
     
   - *Source Code Management:*
      - Repository: `https://github.com/nageshgadupudidemo/linkfire.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `ansible-server`
       - `Transfers` >  `Transfer set`
           - Source files: `webapp/target/*.war`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `//opt//docker`
	       - Exec command: 
                ```sh 
                ansible-playbook -i /opt/docker/hosts /opt/docker/linkfire-image.yml --limit localhost;
                ```

Save and run the job.

### Steps to create "Deploy_on_Kubernetes_CI" Jenkin job
	 
  - *Post-build Actions*  
    - Send build artifacts over SSH  
      - *SSH Publishers*  
	       - Exec command: 
                ```sh 
                ansible-playbook -i /opt/docker/hosts /opt/docker/linkfire-deployment.yml;
                ansible-playbook -i /opt/docker/hosts /opt/docker/linkfire-service.yml;
                ```
Save and run the job.
