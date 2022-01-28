# Tomcat installation on EC2 instance

### Pre-requisites
1. EC2 instance with Java 11


### Install Apache Tomcat
1. Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
  
   ```sh 
   # Create tomcat directory
   cd /opt
   wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.58/bin/apache-tomcat-9.0.58.tar.gz
   tar -xvzf /opt/apache-tomcat-<version>.tar.gz
   ```
2. give executing permissions to startup.sh and shutdown.sh which are under bin. 
   ```sh
   chmod +x /opt/apache-tomcat-<version>/bin/startup.sh 
   chmod +x /opt/apache-tomcat-<version>/bin/shutdown.sh
   
   
   chmod +x /opt/tomcat/bin/startup.sh 
   chmod +x /opt/tomcat/bin/shutdown.sh
   ```
3. create link files for tomcat startup.sh and shutdown.sh 
   ```sh
   ln -s /opt/apache-tomcat-<version>/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/apache-tomcat-<version>/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   
   ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   ```
  
#### Issues & Work Arounds :

Access tomcat application from browser on port 8080  
 - http://<Public_IP>:8080

1. now application is accessible on port 8080. but tomcat application doesnt allow to login from browser. 
changing a default parameter in context.xml does address this issue
   ```sh
   #search for context.xml
   find / -name context.xml
   ```
   ```sh 
   /opt/tomcat/webapps/host-manager/META-INF/context.xml
   /opt/tomcat/webapps/manager/META-INF/context.xml
   
   # Restart tomcat services
   tomcatdown  
   tomcatup
   ```
3. Update users information in the tomcat-users.xml file
goto tomcat home directory and Add below users to conf/tomcat-users.xml file
   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
   ```
4. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful

