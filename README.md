Sample Code to deploy to Azure Container Instance or Webapp using Azure Container Registry/images.

Prerequities - Install Azure Toolkit and Docker plugins for IDE. For me it's IntelliJ. Install Docker Desktop or Docker. For me Docker Desktop.


1-Create a Azure Container Registry.

2-Create a sample Spring Boot Repo like this.

3-Install Azure Toolkit and Docker plugins for IDE. 

4-Open project in IntelliJ and right click on the root folder ----- >>  Azure ----> Docker Support

5- You should see a docker file created similar to the one attached in this repo.

6- Build docker image using  --  **docker build -t hello-world** .     (make sure to be at the same dir where dockerfile is present)

7- Run the image in a container and try to test it hitting localhost url -- I have docker desktop installed hence didn't use the command and instead use the application to run the image on a container.

8- Now we have to push the image to the container registry we created using the below series of commands. If it's your first time we have to do few one time steps initially to generate gpg key

GPG key generation one time :- 
 
**gpg --generate-key
**

**gpg --list-secret-keys
**

**pass init <generated gpg-id public key>
** 
 
9- First go to container registry in azure portal and click "access keys" and Enable Admin user(bydefault false). Azure will provide a username and 2 passwords   

10- Run below commands to push image to container registry 
  
mvn clean install to build the project and create a jar in target folder
  
**az login** (Login and output should show your subcriptions)
 
**az acr login --name myregistry.azurecr.io** (Output - Login Succeeded)
 
**docker tag hello-world myregistry.azurecr.io/hello-world**  (No output no error)
 
**docker push myregistry.azurecr.io/hello-world**    (some uploading will happen and end sha something will print)

  
11- Verify registry - repository - the image should appear hello-world 
  
12- Create container instance on Azure portal and image source should be azure container registry and select your image
  
**13- On networking section, give a dns name label, and most imp is to add port 8080 - TCP ---- This is so because we have a spring boot application running at default port 8080 and we want to hit this port to get a sample response.**
  
14- Copy the ip followed by :8080 ex- 192.192.192.192:8080 or also the FQDN:8080 and voila we can see the output    
  
  
15- Similarly we can create a webapp from the image from azure registry and we can run the application 

Reference :-  
Push/Pull image to container registry - https://learn.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli

GPG generation --- https://stackoverflow.com/questions/71770693/error-saving-credentials-error-storing-credentials-err-exit-status-1-out
  
