Sure, here are the steps to run a Metabase app using a Docker container in an Ubuntu EC2 instance:

### Prerequisites
1. **EC2 Instance**: Ensure you have an Ubuntu EC2 instance up and running.
2. **Security Group**: Open the necessary ports (usually 80 for HTTP and 3000 for Metabase).

### Steps

1. **Connect to Your EC2 Instance**:
   - Use SSH to connect to your EC2 instance.
     ```bash
     ssh -i your-key.pem ubuntu@your-ec2-public-ip
     ```

2. **Update Packages**:
   - Update the package list and install any available upgrades.
     ```bash
     sudo apt-get update
     sudo apt-get upgrade -y
     ```

3. **Install Docker**:
   - Install Docker by following these commands:
     ```bash
     sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     sudo apt-get update
     sudo apt-get install -y docker-ce
     ```

4. **Install Docker Compose**:
   - Download and install Docker Compose.
     ```bash
     sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```

5. **Create a Directory for Metabase**:
   - Create a directory where you will store your `docker-compose.yml` file.
     ```bash
     mkdir metabase
     cd metabase
     ```

6. **Create the Docker Compose File**:
   - Create a `docker-compose.yml` file with the following content:
     ```yaml
     version: '3.1'

     services:
       metabase:
         image: metabase/metabase
         ports:
           - "3000:3000"
         environment:
           - MB_DB_FILE=/metabase-data/metabase.db
         volumes:
           - metabase-data:/metabase-data

     volumes:
       metabase-data:
     ```

7. **Run Docker Compose**:
   - Use Docker Compose to start the Metabase container.
     ```bash
     sudo docker-compose up -d
     ```

8. **Access Metabase**:
   - Open a web browser and go to `http://your-ec2-public-ip:3000`. You should see the Metabase setup screen.

### Explanation

- **Docker Compose File**:
  - `version: '3.1'`: Specifies the Docker Compose file format version.
  - `services`: Defines the services to be run.
  - `metabase`: The service name.
  - `image: metabase/metabase`: Specifies the Docker image to use.
  - `ports: - "3000:3000"`: Maps port 3000 on the host to port 3000 on the container.
  - `environment`: Sets environment variables.
  - `volumes`: Defines the volumes for persistent data storage.
  - `metabase-data`: Named volume for Metabase data.

This setup ensures that Metabase will start in a Docker container and be accessible via your EC2 instance's public IP on port 3000.