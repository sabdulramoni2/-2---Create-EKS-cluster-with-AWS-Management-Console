# Create EKS cluster with AWS Management Console

## **Project Overview**
This project demonstrates the use of AWS console to create EKS cluster. 

---
  
## **Feature**

### **Creates EKS IAM role**
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

## **Diagrammatic Presentation**

#### **Creates EKS IAM role**
 - Go to IAM
 - Select the service you want to give the role to (EKS)
 - Select EKS-cluster as the use case.
 - Attach EKS-cluster policy
   
   ![image](https://github.com/user-attachments/assets/fe38a1d5-4345-4a32-be39-eda53a5bd8fd)


### **Creates VPC for the worker nodes**
 - Why new VPC? Cos EKS cluster needs specific networking configuration.
 - Use AWS cloud formation to create the VPC.
 - Select create stack.
   
   ![image](https://github.com/user-attachments/assets/e2f759c4-f9fc-413f-b797-8c998f8862b2)

 - Use the template file to create the VPC.

   ![image](https://github.com/user-attachments/assets/7acf1462-9488-4fed-bfac-5478e5b34297)


#### **Create EKS cluster (control plane)**
 - Name your cluster, select k8 version, select encryption.
 - Select your VPC, select the SG of the VPC.
 - Cluster endpoint access (Select public). Public = accessible from outside the VPC.

        ![image](https://github.com/user-attachments/assets/1f4cd403-59ea-4287-a587-5ade3a075cc2)


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

  ![image](https://github.com/user-attachments/assets/afdd7165-467e-4c63-9e0b-ebba8bd7cb4b)
  

### **Create EC2 IAM role for node Node Group**

- Create role for EC2 service.
- Assign 3 policies to the EC2 service:  Worker Node Policy, Container Registry, EKS_CNI_Poilcy.


### **Creates Node group and attach to cluster**

- Go to EKS, select compute and name the Node group.
- Select the role above for the Node group.
- Specify the specification for the EC2 instances: image type, instance type, capacity type etc.
- Select the network configuration, key pair and SG.
- Create the Node group.

  ![image](https://github.com/user-attachments/assets/63eae7c8-1c53-46f5-b9a2-b00e8fb8886e)

  ![image](https://github.com/user-attachments/assets/e059c6b1-c518-43cb-81b0-caac3026d72a)

  ![image](https://github.com/user-attachments/assets/847af745-857b-4d4c-8d34-a5850f6193f6)

  ![image](https://github.com/user-attachments/assets/aec28b97-124b-4c18-83ea-8dfb5e086575)

  ![image](https://github.com/user-attachments/assets/12042d9e-b689-4d21-be47-b8cc801f4dff)



### **Configure auto-scaling for the cluster**

- Auto scaling group was created automatically from above.
- But this doesn’t automatically scale the resources for us yet.
- Now we must configure the k8 autoscaler with auto scaling group.
  
  ![image](https://github.com/user-attachments/assets/338683c4-04c4-425e-b3b7-d3c846407718)

- Create a custom role for the EC2 instance to use the auto scaling.
- Created new Policy for Auto-Scaling Permission.
- Attach the new policy to the node group role.

   ![image](https://github.com/user-attachments/assets/ea51cd3f-452d-4fb5-ad9d-f08910bca809)
  

- The k8 autoscaler use tags to identify the AWS auto scaling group.

  ![image](https://github.com/user-attachments/assets/ea2ad981-76fb-478e-aa92-2b74ae58a9fb)

  ![image](https://github.com/user-attachments/assets/cb0c42eb-85cf-4faf-930a-d2a0de3f3c7c)


- Deployed Auscaler Component in EKS cluster.
     -  Edit the deployment to add; cluster-autoscaler.kubernetes.io/safe-to-evict: ‘false
     - name of the cluster
     - balance-similar-node-groups,
     - skip-node-with-system-pods=false,
     - version (version needs to be the same as the k8 version running in the cluster).

### **Deploy application to EKS cluster**
- Deployed a nginx application with load balancer external service.

  ![image](https://github.com/user-attachments/assets/091f3f9f-ce68-45e0-aa3f-4e3e85618dad)

- Access the application from the load balancer.

  ![image](https://github.com/user-attachments/assets/338a9f44-71c6-440c-a93b-54baed3165f7)


  ![image](https://github.com/user-attachments/assets/133822ad-0c7b-483d-b2d5-95a1f3b7ea89)

  ![image](https://github.com/user-attachments/assets/8443d13f-354b-49d0-b529-fc26f5636343)

  ![image](https://github.com/user-attachments/assets/c083b7cc-2dea-4b6d-a36e-ba6959c01e91)

  ![image](https://github.com/user-attachments/assets/0e51b3b8-2061-4fa5-9e5e-89feae8ed36d)
  
- Started 20 Pods - see autoscaling in action

  ![image](https://github.com/user-attachments/assets/fc1291fd-cf3f-4314-aad0-3008e458ae8d)






