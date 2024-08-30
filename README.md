We will be using Kubeadm cluster on 3 EC2 instaces on which I have CSI installed already !  

I will Also be using MobaXterm Cli to interact with the master Node on AWS !


First thing to do is to create the MongoDB secret that will contain username and password of the mongo database, then i will create the StorageClass , pvc to describe the EBS volume on AWS 
then I Created the deployement of the MongoDB which will also create automatically the EBS volume on AWS and the PV .
Later we will create a MongoDB service and it will be a ClusterIP type to assure inner connection of the MongoDB 
Second Step : I created a Config Map to attach both apps to the MongoDB , which will connect to the MongoDB svc .
Then Create a MongoExpress deployment and attach it to the ConfigMap .
Finally a service for the MongoExpress to expose it and making it possible to connect to it so the svc need to be a LoadBalancer (Or nodePort).

*All machines have an attached role to use EBS already*

First : I created a directory for the project and create a secret in it , and Encrypting the username and the password and putting them in the secret yaml file :

![image](https://github.com/user-attachments/assets/ffe394bf-0ebb-4b2b-ac87-e5db8892ac40)

![image](https://github.com/user-attachments/assets/35e421be-f969-42a6-8436-a72d3c37582a)

![image](https://github.com/user-attachments/assets/17cd199b-0c44-4020-936a-36d74ea330f4)

Then Creating a StorageClass : 

![image](https://github.com/user-attachments/assets/5ca3ae14-0111-43b0-9bb3-63db4896619b)

![image](https://github.com/user-attachments/assets/3059ae4d-cbbd-435d-a6f2-37e448e6c294)

Then Creating PVC : 

![image](https://github.com/user-attachments/assets/7e82bc79-6327-4209-a4cd-449b6fc29d69)

![image](https://github.com/user-attachments/assets/ab8f0bc4-5099-4895-916b-47b49e25a855)

*Volume is pending because the VolumeBindingMode = WaitForFirstConsumer*

Then Creating a Deploymenet : 

Upon applying the yaml file of the deploy , the PV will be created and the EBS volume on AWS will be created automatically also : 

The Deployment will be pointing on the username and password from the secret yaml file 

![Capture d'écran 2024-08-30 175953](https://github.com/user-attachments/assets/23084086-f779-499e-a6f4-8011f622eb44)

![image](https://github.com/user-attachments/assets/69625304-28aa-44e3-88ac-471ec5e53b7a)

after some minutes the deployment is ready

![image](https://github.com/user-attachments/assets/f7b3db7d-76af-480d-978e-017802468eb1)

And a Pod is created ofc : 

![image](https://github.com/user-attachments/assets/411a86ad-d045-4e4e-b112-eaa20cca1bca)

And Also an EBS volume is created automatically : 

![image](https://github.com/user-attachments/assets/1614bf44-3062-45c2-9ade-14ef102c5bba)

Note that the PVC is Bound now : 

![image](https://github.com/user-attachments/assets/c2c4df24-3a1f-488d-8697-aacc7d51e949)

And the PV is created automatically also with the EBS :

![image](https://github.com/user-attachments/assets/db39d18e-9af6-424f-b404-eac594c4aed7)

Next step is creating the Service of the mongoDB (ClusterIP) for inner traffic : 

![image](https://github.com/user-attachments/assets/d9452c1a-6768-45ef-a5c4-a6c410146af2)

![image](https://github.com/user-attachments/assets/b867a845-9e8c-4cdd-9cc8-69a442b030eb)

Verifying the Cluster with the KubeView : 

![image](https://github.com/user-attachments/assets/0c41fa5f-50df-4963-aed5-7fbfcf14ab19)

Then Creating a Config map :

*Its connecting to the MongoDB service for the moment because we didnt create the mongoExpress Deploymenet yet* 

![image](https://github.com/user-attachments/assets/fb8b10cf-8c6f-4a05-9995-caec45ad8b21)

![image](https://github.com/user-attachments/assets/96d50b96-4c0d-4f60-868f-d554229611c6)

Now we will create the deployment of the mongoExpress : 

*It will connect to the mongoDB secret and the config Map*

![Capture d'écran 2024-08-30 182053](https://github.com/user-attachments/assets/03bf1543-90de-49ee-a73e-863b973b9c90)

*8081 is default mongoExpress port*

So we have the mongoExpress pod running now : 

![image](https://github.com/user-attachments/assets/70fac770-1847-4be3-b489-dae8c159927d)

Reviewing The cluster using KubeView : 

![image](https://github.com/user-attachments/assets/f90175ed-630a-4afe-a88a-21e43bbe9a68)

Finally Creating the Service for the mongoExpress to be exposed ! : 

![image](https://github.com/user-attachments/assets/bbc0c340-74a6-4174-9313-1708ba13c15d)

![image](https://github.com/user-attachments/assets/2e5be24b-ca36-4636-b6f4-9bab57ed02ca)

*31000 is the port To acces the mongoExpress Now*

Using a Public EC2 instance ip + 31000 to connect : 

![image](https://github.com/user-attachments/assets/a9f77e2a-ede9-478a-83c4-d959c8402dd4)

*Success*

![image](https://github.com/user-attachments/assets/ed83515a-97e9-4a04-88ab-bbe57b275a71)




