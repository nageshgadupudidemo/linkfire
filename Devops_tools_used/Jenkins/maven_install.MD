#  Install & configure Maven build tool on Jenkins

#### Prerequisites
1. Jenkins server

#### Install Maven on Jenkins
1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. 
In this case, I am using /opt/maven as my installation directory
 - Link : https://maven.apache.org/download.cgi
    ```sh
     # Creating maven directory under /opt
     mkdir /opt/maven
     cd /opt/maven
     # downloading maven version 3.8.3
     wget http://mirrors.estointernet.in/apache/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
     tar -xvzf apache-maven-3.8.3-bin.tar.gz
     ```
	
2. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven/apache-maven-3.8.3
   M2=$M2_HOME/bin
   PATH=<Existing_PATH>:$M2_HOME:$M2
   ```

### Setup maven on Jenkins GUI console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`

