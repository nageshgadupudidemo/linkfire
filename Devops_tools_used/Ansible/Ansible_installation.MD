# Ansible Installation


### Pre-requisites

1. An AWS EC2 instance 

### Installation steps:
#### on Amazon EC2 instance

1. Install python and python-pip
   ```sh
   yum install python
   yum install python-pip
   ```
2. Install ansible using pip check for version
    ```sh
    pip install ansible
   ansible --version
   ```
   
3. Create a user called ansadmin 
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
 
4. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
5. Copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>
   ```

6. Ansible server used to create images and store on docker registry. 
Hence install docker, start docker services and add ansadmin to the docker group. 

   ```sh
   yum install docker
   
   # start docker services 
   service docker start
   service docker start 
   
   # add user to docker group 
   usermod -aG docker ansadmin

   ```
7. Create a directory /etc/ansible and create an inventory file called "hosts" 
add control node and managed hosts IP addresses to it. 
 
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```
