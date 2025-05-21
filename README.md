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

2.	Creates VPC for the worker nodes.
a.	Why new VPC? Cos EKS cluster needs specific networking configuration.
b.	Use AWS cloud formation to create the VPC.
c.	Select create stack.
d.	Use the template file to create the VPC.

3.	Create EKS cluster (control plane).
a.	Name your cluster, select k8 version, select encryption.
b.	Select your VPC, select the SG of the VPC.
c.	Cluster endpoint access (Select public). Public = accessible from outside the VPC.

4.	Connect to EKS cluster with kubectl from the local terminal.
a.	Create a KUBECONFIG file by running the below command.
b.	“aws eks update-kubeconfig - - name cluster_name”.
c.	File is located in .kube/config.
d.	See file by cat. kube/config.
e.	Run kubectl get nodes to see the nodes in the cluster. 

5.	Create EC2 IAM role for node Node Group.
a.	Create role for EC2 service.
b.	Assign 3 policies to the EC2 service:  Worker Node Policy, Container Registry, EKS_CNI_Poilcy.

6.	Creates Node group and attach to cluster.
1.	Go to EKS, select compute and name the Node group.
2.	Select the role above for the Node group.
3.	Specify the specification for the EC2 instances: image type, instance type, capacity type etc.
4.	Select the network configuration, key pair and SG.
5.	Create the Node group.

7.	Configure auto-scaling for the cluster.
a.	Auto scaling group was created automatically from above.
b.	But this doesn’t automatically scale the resources for us yet.
c.	Now we must configure the k8 autoscaler with auto scaling group. 
d.	Create a custom role for the EC2 instance to use the auto scaling.
e.	Created new Policy for Auto-Scaling Permission.
f.	Attach the new policy to the node group role.
g.	The k8 autoscaler use tags to identify the AWS auto scaling group.
h.	Deployed Auscaler Component in EKS cluster.
i.	Edit the deployment to add “cluster-autoscaler.kubernetes.io/safe-to-evict: ‘false”, “name of the cluster”, - -- balance-similar-node-groups, - --skip-node-with-system-pods=false, version. Version needs to be the same as the k8 version running in the cluster.

8.	Deploy application to EKS cluster.
a.	Deployed a nginx application with load balancer external service.
b.	Access the application from the load balancer.

