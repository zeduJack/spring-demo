# Local installation (docker desktop)

1. List all gradle tasks ./gradlew tasks --all
2. ./gradlew clean build docker generateDockerCompose
   1. Clean the previous build, 
   2. create a new build your application, 
   3. create docker image, 
   4. create docker-compose 
3. Start your docker-compose ./gradlew dockerComposeUp
4. Check if the image is running docker ps
5. Stop the containers ./gradlew dockerComposeDown

# Synology NAS installation
1. Copy the .jar file (e.g. spring-demo-0.0.1-SNAPSHOT.jar) from build/libs to a folder in the NAS
2. Copy the Dockerfile to the same location where your .jar file is
2. SSH to the NAS
   1. In the disk station got to "Control Panel/Terminal & SNMP/Terminal" and check "Enable SSH Service" (leave the port as it is: 22)
   2. Go in the terminal and type "ssh <username>@<ip> -p 22"
3. Go to the Location where your artefacts are (step 1 & 2)
4. Type "sudo docker build -t <tag-name> ." e.g: "sudo docker build -t localhost/spring-demo ."
5. Start the container in your DSM
   1. Go to your docker and find the image in the "Image" left hand navigation
   2. Doubleclick the image
   3. Click "Advanced Settings"
   4. Go to the tab "Port Settings"
   5. Click "Add"
   6. Set Container port to "8080"
   7. Leave Local Port to "Auto" or your fixed preferred port
8. Open your browser with this URL: "<your DSM IP>:<port from 5.7"
9. If you have your on DDNS set up and want to access it with a subdomain over 443, then you can set up the reverse proxy:
   1. Go to "Control Panel/Login Portal/Advanced/Reverse Proxy"
   2. Click "Create"
   3. Source settings:
      1. Reverse Proxy Name: <any name>
      2. Protocol: HTTPS 
      3. Hostname: <your hostname with subdomain> -> e.g. spring.example.com 
      4. Port: 443
   4. Destination settings
      1. Protocol: HTTP
      2. Hostname: <your DSM IP address>
      3. Your port form 5.7
   4. Create certificate for the subdomain
      1. Go to "Control Panel/Security/Certificate"
      2. Click "Add"
      3. Choose "Add a new certificate" (default) and click "Next"
      4. Choose "Get a certificate form Let's Encrypt"
      5. In "Domain name:" put your domain with subdomain: e.g.: spring.example.com
      6. In "Email:" put your email address
      7. Click "Done"
      8. Now you should be back at Go to "Control Panel/Security/Certificate"
      9. Click on "Settings" and search on the left hand side your Service (hostname from 9.3.3) e.g. spring.zedu.ch and select on the right hand side your certificate with the same name