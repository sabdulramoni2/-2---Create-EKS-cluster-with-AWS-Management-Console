# Deploying MongoDB and Mongo Express

## **Project Overview**
This project demonstrates the use of AWS console to create EKS cluster. 

---

## **Prerequisites**
- Minikube cluster running
  
## **Deploying MongoDB and MongoExpress**
- Created MongoDB Deployment
  -	Configure the deployment file to include port 27017 which is MongoDB port
  - Added env variable with name and values for the username and password.
  - The values will be in the secret config file
-	Created secret config file
     - The value of the secrets (username and password) is base 64 encoded
       ```
           echo -n ‘username’ | base64  =  values will be paste in secret file
           echo -n ‘password’ | base64  =  values will be paste in secret file
       ```
     - Always create secret before Deployments since you want Deployment to reference it.
     - The secret is reference using the “Value from” “secretKeyRef” 
- Create secret and then deployment
- Create service config file for MongoDB
- Created MongoExpress Deployment
