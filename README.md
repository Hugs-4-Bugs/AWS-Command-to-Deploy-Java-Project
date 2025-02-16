# Deployment Instructions for Ollama Spring Boot Application

This README outlines the full process for deploying your Spring Boot application using Ollama on an EC2 instance, including model pulling and running commands.

---

### 1. **Connect to EC2 Instance:**

* **SSH into EC2 instance** using the `.pem` key:
    ```bash
    ssh -i "PrabhatGPT.pem" ec2-user@<your-ec2-public-ip>
    ```

* **Set permissions for `.pem` key (if needed)**:
    ```bash
    chmod 400 PrabhatGPT.pem
    ```

---

### 2. **Update and Install Dependencies on EC2:**

* **Update the system**:
    ```bash
    sudo yum update -y
    ```

* **Install Java** (e.g., OpenJDK):
    ```bash
    sudo yum install java-19-openjdk-devel -y
    ```

* **Verify Java version**:
    ```bash
    java -version
    ```

* **Install Maven** (if not already installed):
    ```bash
    sudo yum install maven -y
    ```

* **Install Git** (if not already installed):
    ```bash
    sudo yum install git -y
    ```

* **Install Docker** (for containerized deployment):
    ```bash
    sudo yum install docker -y
    sudo service docker start
    sudo usermod -aG docker ec2-user
    ```

* **Install Ollama**:
    ```bash
    curl -sSL https://ollama.com/install | bash
    ```

* **Install DeepSeek and Misral** (if applicable):
    ```bash
    # For DeepSeek
    curl -sSL https://deepseek.ai/install | bash

    # For Misral (if applicable)
    curl -sSL https://misral.ai/install | bash
    ```

---

### 3. **Clone Your Project from GitHub:**

* **Clone your Spring Boot project**:
    ```bash
    git clone https://github.com/yourusername/ollama-spring-boot-implementation.git
    cd ollama-spring-boot-implementation
    ```

---

### 4. **Build Your Spring Boot Application Locally:**

* **Maven build** (this generates the JAR file):
    ```bash
    mvn clean install
    ```

---

### 5. **Deploy the Application on EC2:**

* **Copy the JAR file to your EC2 instance** (if needed):
    ```bash
    scp -i "PrabhatGPT.pem" target/ollama-spring-boot-implementation-0.0.1-SNAPSHOT.jar ec2-user@<your-ec2-public-ip>:/home/ec2-user/
    ```

* **Navigate to the directory on EC2 where the JAR file is located**:
    ```bash
    cd /home/ec2-user
    ```

* **Run the application on EC2**:
    ```bash
    java -jar ollama-spring-boot-implementation-0.0.1-SNAPSHOT.jar
    ```

* **(Optional)** To run the application in the background:
    ```bash
    nohup java -jar ollama-spring-boot-implementation-0.0.1-SNAPSHOT.jar &
    ```

---

### 6. **Ollama-Specific Commands:**

* **Pull DeepSeek model** (latest version):
    ```bash
    ollama pull deepseek
    ```

* **Pull Mistral model** (latest version):
    ```bash
    ollama pull mistral
    ```

* **List installed models**:
    ```bash
    ollama list
    ```

* **Run DeepSeek model**:
    ```bash
    ollama run deepseek-r1:latest
    ```

* **Run Mistral model**:
    ```bash
    ollama run mistral:latest
    ```

---

### 7. **Configure Security Groups on EC2:**

* Open port 8180 (or whichever port your app uses) in the EC2 security group to allow traffic:
    ```bash
    aws ec2 authorize-security-group-ingress --region <region> --group-id <security-group-id> --protocol tcp --port 8180 --cidr 0.0.0.0/0
    ```

---

### 8. **Test the Application:**

* Access your application via the public IP and port (e.g., `http://<your-ec2-public-ip>:8180`).

---

### 9. **(Optional) Run Dockerized Version (If Using Docker):**

* Build the Docker image:
    ```bash
    docker build -t ollama-spring-boot-app .
    ```

* Run the Docker container:
    ```bash
    docker run -d -p 8180:8080 ollama-spring-boot-app
    ```

---


