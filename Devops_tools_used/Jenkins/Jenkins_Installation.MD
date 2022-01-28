# Install Jenkins on AWS EC2

### Prerequisites
1. EC2 Instance 
   - Security Group with Port `8080` open for internet
2. Java 11 should be installed  


## Install Jenkins
 
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
   amazon-linux-extras install epel 
   amazon-linux-extras install java-openjdk11  
   yum install jenkins
   ```

   ### Start Jenkins
   ```sh
   # Start jenkins service
   service jenkins start

   # Setup Jenkins to start at boot,
   chkconfig jenkins on
   ```

   ### Accessing Jenkins
   By default jenkins runs at port `8080`, access jenkins at
   ```sh
   http://JENKINS-EC2_INSTANCE-PUBLIC-IP:8080
   ```
  #### Configure Jenkins GUI
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  


