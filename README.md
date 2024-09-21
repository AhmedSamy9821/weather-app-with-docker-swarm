Here's the updated README with the steps swapped as requested:

---

# Weather App with Docker Swarm

## Overview

This project is a weather application deployed using Docker Swarm. The application architecture includes two containers for each component: UI, weather, and auth, ensuring high availability and resilience. A MySQL database is used for data storage, and a volume is configured for data persistence. 

## Project Components

- **UI Containers**: Handle user interactions and display weather information.
- **Weather Containers**: Fetch and process weather data from external APIs.
- **Auth Containers**: Manage user authentication and session management.
- **MySQL Database**: Stores application data with a configured volume for persistence.

## Deployment Steps

To deploy this project using Docker Swarm, follow these steps:

1. **Create Manager and Worker Nodes**:
   - Set up instances for the manager and worker nodes.

2. **Install Docker**:
   - Install Docker on all instances (manager and workers).

3. **Initiate Docker Swarm Cluster**:
   - Initialize the Docker Swarm cluster on the manager node:
     ```bash
     docker swarm init --advertise-addr <MANAGER-IP>
     ```
   - Join the worker nodes to the cluster using the command provided by the manager.

4. **Allocate Elastic IP to Manager Node**:
   - Allocate an Elastic IP to the manager node for consistent access.

5. **Prevent Manager from Acting as a Worker**:
   - Limit the manager node to manage the cluster without running tasks:
     ```bash
     docker node update --availability drain <MANAGER-NODE-NAME>
     ```

6. **Clone the Application Repository**:
   - Clone the application repository:
     ```bash
     git clone https://gitlab.com/abohmeed/moderndevops-weatherapp.git
     cd moderndevops-weatherapp
     ```

7. **Build and Push Docker Images**:
   - Build and push your Docker images to Docker Hub:
     ```bash
     docker build -t your-dockerhub-username/weather-app .
     docker push your-dockerhub-username/weather-app
     ```

8. **Write the `docker-compose.yml` File**:
   - copy`docker-compose.yml` file in this repository and put it within the repository directory.

9. **Deploy the Services**:
   - Deploy the stack using Docker Compose:
     ```bash
     docker stack deploy --compose-file=docker-compose.yml weather-app
     ```

10. **Create DNS Record in Route 53**:
    - Create a DNS record in Route 53 to redirect your domain to the Elastic IP of the manager node.

## Additional Information

- **High Availability**: The use of multiple containers for each component ensures that the application remains available even if individual containers fail.
- **Data Persistence**: The MySQL database is configured with a volume to ensure that data is not lost even if the containers are restarted or redeployed.
- **Scalability**: The application can be easily scaled by increasing the number of replicas for each service in the Docker Swarm configuration.

