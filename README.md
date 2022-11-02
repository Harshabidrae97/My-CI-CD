##### build the project

    ./gradlew build

##### build Docker image called java-app. Execute from root

    docker build -t java-app .
    
##### push image to repo 

    docker tag java-app demo-app:java-1.0
    
Set Up a CI/CD Pipeline

Reducing the deployment time was another major pain point that had to be addressed, and Tech Mahindra used an automated CI/CD pipeline to mitigate that.

To set up the CI/CD pipeline, the team used Jenkins for continuous integration and continuous deployment, with Maven for building. JFrog Artifactory stores Maven-built .jar files and is a universal binary repository manager where you can manage multiple applications, dependencies, and versions in one place.

Artifactory also enables you to standardize the way you manage package types across all applications developed in your company, no matter the code base or artifact type.

Amazon EC2 is used to host Jenkins and JFrog Artifactory in Docker containers. Running Jenkins in Docker containers allows the more efficient use of servers running Jenkins worker nodes. It also simplifies the configuration of the worker node servers. Using containers to manage builds allows the underlying servers to be pooled into a cluster. The Docker image is built with the .jar files, stored in ECR, and deployed in Amazon ECS as a Fargate deployment.

During new deployment/release, the new Docker image is launched first in Fargate and attached to the Application Load Balancer. After it reaches a healthy state, the old Docker container is drained. It’s later removed from the load balancer after all the connections are drained, thus maintaining zero downtime during deployment and releases.
