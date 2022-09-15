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

  
  7. Now, check the status of jenkins service using ```sudo systemctl status jenkins```.
  
#### Step-2: Setting Up Jenkins

- To set up your installation, visit Jenkins on its default port, 8080, using your server domain name or IP address: ```http://your_server_ip_or_domain:8080```

  You should receive the Unlock Jenkins screen, which displays the location of the initial password:

![image](https://user-images.githubusercontent.com/42695637/190149938-d9a9b447-312b-40a3-bc1a-efdbdf0534c0.png)

- In the terminal window, use the cat command to display the password:

  ```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```
  
  Copy the 32-character alphanumeric password from the terminal and paste it into the Administrator password field, then click Continue.

- The next screen presents the option of installing suggested plugins or selecting specific plugins:

![image](https://user-images.githubusercontent.com/42695637/190150451-8d2339d0-be7b-43af-b367-b575ff553058.png)

  We’ll click the Install suggested plugins option if we want otherwise cancel the same and add plugins according to requirement like ssh for     
  authentication of manage nodes, which will immediately begin the installation process.
  
- When the installation is complete, you’ll be prompted to set up the first administrative user. It’s possible to skip this step and continue as admin       using the initial password we used above, but we’ll take a moment to create the user.  
 
  ![image](https://user-images.githubusercontent.com/42695637/190151358-a53f1c80-08f3-4a52-9a0f-4ffbfe44bbe8.png)

- Enter the name and password for your user and save and continue 
  
- After confirming the appropriate information, click Save and Finish. You’ll receive a confirmation page confirming that “Jenkins is Ready!”
 
- Now, click on start using jenkins.

- Now, we have to install SSH plugin for connectivity between master node and slave node with SSH.

- We have to click on manage node in jenkin and click on new node

  ![image](https://user-images.githubusercontent.com/42695637/190152554-c2312226-aeda-4bdf-93c4-1898ec3354c7.png)

- Give the name and choose permanent agent. After that click on create  
  ![image](https://user-images.githubusercontent.com/42695637/190152816-06c01ff1-ab32-4bed-b44d-296c520936d8.png)
  
- Now fill the details like below:

  ![image](https://user-images.githubusercontent.com/42695637/190153218-6f29a9b2-cb14-48fa-829a-c67de10d3819.png)

  ![image](https://user-images.githubusercontent.com/42695637/190153446-436fb833-84b3-45a8-8549-3135dc7e7e70.png)
  
  ![image](https://user-images.githubusercontent.com/42695637/190153590-e5d84952-4f02-4f15-963a-e6d7c7096b49.png)

  ![image](https://user-images.githubusercontent.com/42695637/190153739-d4ee1a4b-9ddf-4720-9e29-13c67af9e744.png)

  Note:- For the credentials which we have seen ubuntu user we got after the other server creation. Once created we can add the credenntials in below path   in jenkins
  
   ![image](https://user-images.githubusercontent.com/42695637/190154370-f67ffab2-723f-4ae4-8d66-4f0a3d78170e.png)
   
- Now click on New Item for creating the Job and choose Pipeline and click on ok and choose GitHub hook trigger for GITScm polling for GitHub Plugin   triggers a one-time polling on GITScm. When GITScm polls GitHub, it finds that there is a change and initiates a build. 

  ![image](https://user-images.githubusercontent.com/42695637/190156166-9508073a-9273-4042-a783-736409337957.png)
  
- We have to give below option, Pipeline script from SCM and choose the file from github with the name Jenkinsfile

  ![image](https://user-images.githubusercontent.com/42695637/190156562-b4de7d4b-dee0-4833-8b3e-df3ea7603444.png)

  ![image](https://user-images.githubusercontent.com/42695637/190156973-c5748947-f003-452b-a6fc-c6a23d04dbf7.png)
  
  ![image](https://user-images.githubusercontent.com/42695637/190157131-62848d79-42cd-44dc-99e9-33394f529202.png)


#### Step-3: Create the second server for react-Js application

- First we need to update the package list run below command:     
   
   ```sudo apt update``` 
   
- Install the java on server with below command:    
   
  ```apt-get install openjdk-11-jdk```

- After that execute the below commands to install Node and npm

  ``` curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -```
  ``` sudo apt-get install gcc g++ make ```
  ```sudo apt-get install -y nodejs ```
  
- Now let’s install Nginx webserver
 
  ```sudo apt install -y nginx```
  
- After installing the web server, we have to create the root folder to serve our files.

  ```sudo mkdir /var/www/jenkins-react-app```
 
- After completing the above steps, we need to create the configuration file for the Nginx server.
 
- Open a text editor of your choice and create the file /etc/nginx/sites-available/jenkins-react-app and paste the below configs.
  ![image](https://user-images.githubusercontent.com/42695637/190160967-f6652bd1-42f2-49e6-9838-92303a7a1462.png)


- Now we have to create a Symlink to the above configuration in /etc/nginx/sites-enabled

  ``` sudo ln -s /etc/nginx/sites-available/jenkins-react-app /etc/nginx/sites-enabled/jenkins-react-app ```
  
- Now start the nginx server
  
   sudo service nginx start
   
- Now put the jenkinsfile in github which we are mentioned in out Item in jenkins
  
  ![image](https://user-images.githubusercontent.com/42695637/190161696-940f8080-0054-472d-a1bf-ae2d120bdad7.png)
  
- To automatically trigger our Jenkins job, we have to add a Github webhook to our repository, let’s go ahead and add it.

  Go to the settings of your Github repository and go to the Webhooks section. Enter the payload URL, i.e. by default http://{external_dns_hostname or IP}:{jenkins port}/github-webhook/
  
  eg: http://ec2–221–186–93–192.compute-1.amazonaws.com:8080/github-webhook/
  or http://publicIP:8080/github-webhook/
  
  ![image](https://user-images.githubusercontent.com/42695637/190162318-1d726f6a-ca52-47cd-b1dc-de08bc782cc2.png)


- Put the SSH credentials for connectivity node in credentials managers and put the same on connectivity with manage node like below

  ![image](https://user-images.githubusercontent.com/42695637/190162752-7da5c6a4-95d4-4674-b170-d1dfd5d8f0fb.png)


- Now one both nodes are sync as below and after that click on build for manually run. 

  ![image](https://user-images.githubusercontent.com/42695637/190163053-c5147739-141b-470e-8bcf-98e0fe24a2b7.png)

- Once it done after that we need to purchase domain and update A record related to react js server.



#### Step-4: Install letsencrypt certbot SSL on website 

- The first step to using Let’s Encrypt to obtain an SSL certificate is to install the Certbot software on your server.

  Install Certbot and it’s Nginx plugin with apt:  
  
  ``` sudo apt install certbot python3-certbot-nginx ```
  
- After that run below command for installing SSL 

  ``` sudo certbot --nginx -d example.com -d www.example.com ``` 
  
- It will modified the nginx configuration according to SSL. Below are the new conf file for nginx related to website
   
 ![image](https://user-images.githubusercontent.com/42695637/190164646-3929eb2e-042d-4bb2-99c9-75bcf40cc47f.png)

 ![image](https://user-images.githubusercontent.com/42695637/190164760-b5de0fea-b077-4db8-8613-1d5c910f5863.png)
 
 
#### Step-5: Run the Reactjs with Docker now 

- Create the docker file as like below for running the reactJS with the name docker file

  FROM node:16.13.0 AS build

  # Create and set the working directory on the container
  # then copy over the package.json and package-lock.json
    WORKDIR /frontend
    COPY package*.json ./

  # Install the node packages before copying the files
    RUN npm install


    COPY . .

  # build your app
    RUN npm run build

  # production environment
    FROM nginx:1.17.4-alpine
    COPY --from=build /frontend/build /usr/share/nginx/html
    RUN rm /etc/nginx/conf.d/default.conf
  # change the left path with yours, below the file content
    COPY --from=build /frontend/nginx.conf /etc/nginx/conf.d
    EXPOSE 80
    CMD ["nginx", "-g", "daemon off;"]
    
 -  Now create nginx file in same directory where the docker file is present and also you have files of reactjs on same directory
 
    server {

             listen 80;

             location / {
               root   /usr/share/nginx/html;
               index  index.html index.htm;
               try_files $uri $uri/ /index.html;
             }

            error_page   500 502 503 504  /50x.html;

            location = /50x.html {
            root   /usr/share/nginx/html;
           }

   }
   
   ![image](https://user-images.githubusercontent.com/42695637/190319921-345aa841-4e4f-4ab0-9246-9a7994caacdf.png)
   
-  Now run the command for build the image from dockerfile
  
   ```docker build -t reactjs:latest .```
             or
   ```docker build -t imagename:tag .```
   
- After that run the container with below command from that image:

  ``` docker container run -d -p 80:80 --name reactapp reactjs:latest```
                       or
  ``` docker container run -d -p 80:80 --name containername createdimagename```
  
  
- Now push the image in dockerhub so that everyone will get it.

  First login into dockerhub with below command and enter the credentials after that we can push the image in dockerhub
  
  ```docker login```
  
  Before push we need to rename the tag according to our account
  
  docker tag reactjs mak1993/reactjs
  
  For push the image in dockerhub run below command:
  
  ```docker push mak1993/reactjs:latest``` 

    
    


    
 
  



  



  
