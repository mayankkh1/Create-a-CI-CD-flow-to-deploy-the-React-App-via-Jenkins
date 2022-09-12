## Description:

- Create 2 servers.
- Use 1st as a Jenkins server and 2nd as an app server.
- Create a sample react app and deploy it on the app server.
- Upload that code in the GitHub Repository.
- Make a Jenkins Pipeline and deploy the code to the server.
- Configure Nginx to configure the domain and SSL to serve the website.

## Acceptance Criteria:
  
- The website should be accessible by the domain and should open on HTTPS.
- Whenever any changes will be pushed to the repository, they should deploy on the app server with zero downtime.


#### Step-1: Launch 2 EC2 server, 1st for Jenkins and 2nd for App deployment:

- First login into AWS account with below URL:

  ![image](https://user-images.githubusercontent.com/42695637/189657643-f3d15862-bc73-4e1e-ba9a-4974351b48b7.png)
  
- After login, navigate to the search bar, type EC2, and select EC2

  ![image](https://user-images.githubusercontent.com/42695637/189658124-8e56a5f8-2cad-4925-a994-b29625a9a2c8.png)
  
- Now, the EC2 dashboard will appear. Click on Instances and after that click on Launch instances
 
  ![image](https://user-images.githubusercontent.com/42695637/189660093-7d6819b7-9ca8-43be-8ad4-f3b8cc74b908.png)

- After that choose the Image and given the details according to requirements like security group , VPC etc.

### Step-2: On 1st server install and configure Jenkins:

- Now, take the public IP from the EC2 dashboard and use it to login inside the instance using ssh.
  
  ![image](https://user-images.githubusercontent.com/42695637/189661668-b1407819-e87f-47a4-8737-7d68f9f45bf4.png)

- After logging in, first update local package index using ```sudo apt update```

- Jenkins require jdk to run so let's install jdk using ```apt-get install openjdk-11-jdk```

- To check jdk is successfully installed or not use ```java -version```

  ![image](https://user-images.githubusercontent.com/42695637/189662911-b2852697-50f9-493f-a08c-ea6944b729fc.png)
  
- Now to install jenkins, follow below steps these are provided on jenkins official website also:

  1. First, add the repository key to the system:
  
  ```wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -```
  
  2. Next, let’s append the Debian package repository address to the server’s sources.list:
  
  ```sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'```
  
  3. After both commands have been entered, we’ll run update so that apt will use the new repository.
  
  ```sudo apt update```
 
  4. Finally, we’ll install Jenkins and its dependencies.
    
  ```sudo apt install jenkins```
  
  5. Let’s start Jenkins by using systemctl:
  
  ```sudo systemctl start jenkins```
  
  6. To check jenkins version use ```jenkins --version```.
  
     ![image](https://user-images.githubusercontent.com/42695637/189666224-a819cf1e-641a-4aa5-9de2-e13f50db71bf.png)

  
-  
