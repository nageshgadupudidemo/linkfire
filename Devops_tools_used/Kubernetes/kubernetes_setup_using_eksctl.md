# Setup Kubernetes on Amazon EKS

Amazon EKS – eksctl(https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)   

#### Pre-requisites: 
  - an EC2 Instance 
  - Install AWSCLI latest verison 

1. Setup kubectl   
   a. Download kubectl version 1.21  
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    

   ```sh 
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version
   ```
2. Setup eksctl   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   

   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
  
3. Create an IAM Role and attache it to EC2 instance    
   
   IAM user should have access to   
   IAM   
   EC2   
   CloudFormation  
   
   eksctl documentaiton : [Minimum IAM policies](https://eksctl.io/usage/minimum-iam-policies/)
   
4. Create your cluster and nodes 
   ```sh
     
   eksctl create cluster --name linkfire  \
   --region us-east-1 \
   --node-type t2.small

    ```

5. To delete the EKS clsuter 
   ```sh 
   eksctl delete cluster linkfire --region us-east-1
   ```
   
6. Validate your cluster using by creating by checking nodes and by creating a pod 
   ```sh 
   kubectl get nodes
   kubectl run tomcat --image=tomcat 
   ```
   
   #### Deploying Nginx pods on Kubernetes
   
1. Deploying Nginx Container
    ```sh
    kubectl deployment linkfire --image=nageshgadupudi1aws/linkfireapp --replicas=2 --port=8080
    kubectl get all
    kubectl get pod
   ```
   
  
2. Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them.
   ```sh
   
   kubectl expose deployment linkfire --port=8080 --type=LoadBalancer
   kubectl get services -o wide
   ```

