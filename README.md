# docker-composePracticeApp
Task 1
First of all launch an EC2 instance with docker and docker-compose installed onto it. Port 80, 8080,443,22 should be open to your IP.

You have to write a docker-compose file (YAML) to deploy the above application using docker-compose such that:

Do not use the 'build' keyword, instead mention the image name of each service.
All services should have one replica.
Map voting frontend service on host port 80 and result-app frontend service on host port 8080.
Deploy your compose file using the docker-compose up command

Verify your work
Try to hit the public IP of your instance from your local computer and ensure that the vote-app frontend is accessible.
Try to hit public IP: 8080 of your instance from your local computer and ensure that the result-app frontend is accessible.
Task 2
The number of voters casting the vote on your website has increased, so you have decided to deploy two replicas of the voting-app frontend. Voting frontend service should have two replicas, rest services should have only one replica. 

Now run docker-compose up --scale vote=2, it will show some error. Remember that two containers cannot be mapped to the same host port, so if you expose port 80 of your voting-app frontend to port 80 of the host then deployment of multiple replicas will show some error. To resolve this use dynamic port mapping. Make changes in the docker-compose file accordingly and deploy your new compose file with docker-compose up --scale vote=2

Verify your work
Run docker ps commands to check if the correct number of replicas of each service is running
Find the dynamically allocated ports for two voting-app containers running on the system.
Task 3
Each voting-app fronted container has a separate port that is also unknown beforehand. You want to set up a load balancer using Nginx that listens on a predefined port 80 and forward the request to one of the containers using the round-robin method of load balancing (which is a default method in case of Nginx).

Create a docker file such that:

Use ubuntu:latest image
Add File author/maintainer
Install Nginx (apt-get update && apt-get install nginx -y)
Replace Nginx virtual configuration with a custom configuration file (path in the container is /etc /nginx/conf.d/default.conf) such that Nginx act as a reverse proxy to a backend service called "voting".
Use the 'Expose' keyword to document that port 80 is to be exposed while running container
use CMD to reload Nginx
Your configuration file should look  

server {
	listen 80;
	listen [::]:80;
	server_name _;
        location / {
            proxy_pass http://vote;
        }
    }
 

Now change your docker-compose file such that:

Add an Nginx service in your compose file, expose its port 80 to host port 80.
This service should build the image during runtime using the "build" keyword such that it uses the dockerfile which you have created above.
Verify your work
When you hit your instance's public IP at port 80, you must be served with a voting-app front end.
As your Nginx is acting as a reverse proxy and load balancing among two containers using round-robin, you will be served by two containers turn-by-turn in each request.
