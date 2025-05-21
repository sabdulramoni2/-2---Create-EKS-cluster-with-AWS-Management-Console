# Create EKS cluster with AWS Management Console

## **Project Overview**
This project demonstrates the use of AWS console to create EKS cluster. 

---
  
### **Feature**

#### **Creates EKS IAM role**
 - Go to IAM
 - Select the service you want to give the role to (EKS)
 - Select EKS-cluster as the use case.
 - Attach EKS-cluster policy

### **Creates VPC for the worker nodes**
 - Why new VPC? Cos EKS cluster needs specific networking configuration.
 - Use AWS cloud formation to create the VPC.
 - Select create stack.
 - Use the template file to create the VPC.

### **Create EKS cluster (control plane)**
 - Name your cluster, select k8 version, select encryption.
 - Select your VPC, select the SG of the VPC.
 - Cluster endpoint access (Select public). Public = accessible from outside the VPC.

### **Connect to EKS cluster with kubectl from the local terminal**
- Create a KUBECONFIG file by running the below command.
    ```
         aws eks update-kubeconfig - - name cluster_name
    ```

- File is located in .kube/config.
- Run the command below to see the file
    ```
         cat. kube/config
         kubectl get nodes
    ``` 

### **Create EC2 IAM role for node Node Group**

- Create role for EC2 service.
- Assign 3 policies to the EC2 service:  Worker Node Policy, Container Registry, EKS_CNI_Poilcy.


### **Creates Node group and attach to cluster**

- Go to EKS, select compute and name the Node group.
- Select the role above for the Node group.
- Specify the specification for the EC2 instances: image type, instance type, capacity type etc.
- Select the network configuration, key pair and SG.
- Create the Node group.

### **Configure auto-scaling for the cluster**
- Auto scaling group was created automatically from above.
- But this doesn’t automatically scale the resources for us yet.
- Now we must configure the k8 autoscaler with auto scaling group.
- Create a custom role for the EC2 instance to use the auto scaling.
- Created new Policy for Auto-Scaling Permission.
- Attach the new policy to the node group role.
- The k8 autoscaler use tags to identify the AWS auto scaling group.
- Deployed Auscaler Component in EKS cluster.
     -  Edit the deployment to add; cluster-autoscaler.kubernetes.io/safe-to-evict: ‘false
     - name of the cluster
     - balance-similar-node-groups,
     - skip-node-with-system-pods=false,
     - version (version needs to be the same as the k8 version running in the cluster).

### **Deploy application to EKS cluster**
- Deployed a nginx application with load balancer external service.
- Access the application from the load balancer.

