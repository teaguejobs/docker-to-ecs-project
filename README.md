# Docker to AWS ECS project
### Run application using docker on EC2 server
## Steps
1. Create an EC2 server on the AWS Console (make sure you open port 80 in your security group)
2. ssh into the server
3. install git using the following command:
    * sudo yum install git
4. clone the code to the server
5. install docker on the server using the following commands:
    * sudo yum update
    * sudo yum install docker -y
    * sudo systemctl start docker
    * sudo usermod -aG docker ec2-user
    * exit (exit from server and ssh again)
6. Create a private ECR repository on the AWS console with any name (like node-react-app)
7. configure your AWS credentials using  the export commands or the aws configure command:
    * export AWS_ACCESS_KEY_ID=your-access-key
    * export AWS_SECRET_ACCESS_KEY=your-secret-key
    * export AWS_DEFAULT_REGION=your-ecr-region
8. go to the ecr repo and copy push commands from ecr and paste in the location where docker file exists for login, build, tagging and push the image to ecr (inside the repository you cloned)
9. after running all commands, check the ecr repo and confirm the image exists
10. Run the docker run command to start the container
    * docker run -d -p 80:80 node-react-app:latest
11. after starting the container, copy the public ip of the EC2 server and paste in a browser to see if the app is working or not.

### Run the application using ECS (Migrating the application to ECS)

1. Create an ECS cluster
    * enter the name
    * select any vpc and subnets
    * select fargate from AWS Infrastructure
2. Create a task definition
    * Enter the name
    * In container 1, enter a container name
    * copy the ecr image url and tag and paste in image URI
    * container port 80
    * select the app environment fargate
3. Create a service inside the cluster
    * select launch type fargate
    * select application type Service
    * in family, select the task definition you created earlier
    * type a service name
    * in the load balancer section, select application load balancer
    * create new and enter a name name
    * select create new target group and enter a name
    * Create the service 
4. Go to load balancer in EC2, copy the dns name and paste in a browser to see the application working.