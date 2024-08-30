This is the overall project Architecture :

![image](https://github.com/user-attachments/assets/0e3ae24b-7498-494b-83b9-7773716f2f6b)

We will be using Kubeadm cluster on 3 EC2 instaces on which I have CSI installed already !  

I will Also be using MobaXterm Cli to interact with the master Node on AWS !


First thing to do is to create the MongoDB secret that will contain username and password of the mongo database, then i will create the StorageClass , pvc to describe the EBS volume on AWS 
then I Created the deployement of the MongoDB which will also create automatically the EBS volume on AWS and the PV .
Later we will create a MongoDB service and it will be a ClusterIP type to assure inner connection of the MongoDB 
Second Step : I created a Config Map to attach both apps to the MongoDB , which will connect to the MongoDB svc .
Then Create a MongoExpress deployment and attach it to the ConfigMap .
Finally a service for the MongoExpress to expose it and making it possible to connect to it so the svc need to be a LoadBalancer ( Or nodePort).

*All machines have attached role to use EBS already*

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













