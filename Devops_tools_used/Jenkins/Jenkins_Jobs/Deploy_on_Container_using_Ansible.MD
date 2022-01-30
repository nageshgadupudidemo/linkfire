# Deploy on a docker container using Ansible

### Pre-requisites

1. Jenkins server 
2. Docker-host server 
3. Ansible server
4. `Dockerfile` under *`/opt/docker`* on Ansible server
   ```sh 
   # Pull tomcat latest image from dockerhub 
   From tomcat
   
   # copy war file on to container 
   COPY ./webapp.war /usr/local/tomcat/webapps

1. Create `create-docker-image.yml` under *`/opt/docker`* on Ansible server
   ```sh
   ---
   - hosts: all
     become: true 

     tasks:
     - name: building docker image
       command: "docker build -t linkfire-image ." 
       args:
         chdir: /opt/docker
   ```
1. Create `create-docker-container.yml` under *`/opt/docker`* on Ansible server
   ```ssh
   ---
   - hosts: all
     become: true

     tasks:
     - name: creating docker container using docker image
       command: docker run -d --name linkfire-container -p 8080:8080 linkfire-image
   ```

### Integration between Ansible-control-node and Jenkins

Install "publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Enable connection between Ansible-control-node and Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers` 

	- SSH Servers:
                - Name: `ansible-server`
		- Hostname:`<ServerIP>`
		- username: `ansadmin`
               
       -  `Advanced` > chose `Use password authentication, or use a different key`
		 - password: `*******`
 
### Steps to create "Deploy_on_Container_using_ansible" Jenkin job

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
                ansible-playbook -i /opt/docker/hosts /opt/docker/create-docker-image.yml;
                ```

Save and run the job now.
