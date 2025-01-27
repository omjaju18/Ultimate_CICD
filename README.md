# CI/CD Pipeline for Ekart Application

This pipeline automates the development, testing, and deployment process for the Ekart application, ensuring efficient and consistent software delivery. Below is an overview of the pipeline stages, the technologies used, and their purposes.

<img width="548" alt="image" src="https://github.com/user-attachments/assets/1018ca0e-7441-463d-991c-c2064291ee12" />

---

## **Technologies Used**
- **Languages and Build Tools:**
  - Java (JDK 21)
  - Maven 6
- **Code Quality Analysis:** SonarQube and Sonar Scanner
- **Containerization:** Docker
- **Source Control:** GitHub
- **CI/CD Orchestration:** Jenkins

## **Commands to Set Up Jenkins and SonarQube**

### Start SonarQube
Run the following command to start a SonarQube instance:

docker run -d -p 9000:9000 sonarqube:lts-community
The SonarQube server will be accessible at: http://localhost:9000.

Start Jenkins
Run the following command to start a Jenkins instance:

docker run -d --name jenkins -p 50000:50000 -v jenkins_data:/var/jenkins_home jenkins/jenkins:lts
Jenkins will be accessible at: http://localhost:8080.
The jenkins_data volume ensures persistent Jenkins data.

## **Pipeline Stages**

### 1. **GitHub Checkout**
   - **Purpose:** Clones the code repository to the Jenkins workspace for the pipeline to work with the latest codebase.
   - **Technology Used:** Git
   - **Key Action:**
     - Checks out the `main` branch from the GitHub repository using the Jenkins Git plugin.

---

### 2. **Compile**
   - **Purpose:** Ensures the source code is free from syntax errors and compiles successfully.
   - **Technology Used:** Maven
   - **Key Action:**
     - Executes `mvn clean compile` to clean previous builds and compile the source code.

---

### 3. **SonarQube Analysis**
   - **Purpose:** Performs static code analysis to ensure code quality, maintainability, and security.
   - **Technologies Used:**
     - SonarQube
     - Sonar Scanner
   - **Key Actions:**
     - Analyzes the source code for issues like bugs, code smells, and security vulnerabilities.
     - Configures project-specific parameters like `sonar.url`, `sonar.projectKey`, and `sonar.login`.

---

### 4. **Build Application**
   - **Purpose:** Packages the compiled application into an executable JAR file.
   - **Technology Used:** Maven
   - **Key Action:**
     - Runs `mvn clean install` to package the application and generate the artifact.

---

### 5. **Build and Push Docker Image**
   - **Purpose:** Creates a Docker image for the application and pushes it to a Docker registry for containerized deployment.
   - **Technologies Used:**
     - Docker
     - Jenkins Docker Plugin
   - **Key Actions:**
     - Builds the Docker image using a `Dockerfile`.
     - Tags the image with the name `omjaju18/shopping:latest`.
     - Pushes the tagged image to Docker Hub.

---

### 6. **Trigger CD Pipeline**
   - **Purpose:** Initiates the downstream Continuous Deployment (CD) pipeline for deploying the application.
   - **Technology Used:** Jenkins
   - **Key Action:**
     - Triggers the `CD_Pipeline` job using the Jenkins `build` command.

---

---

## **How to Run the Pipeline**
1. Ensure that all required tools are installed and configured:
   - Jenkins with necessary plugins (e.g., Git, Docker, SonarQube).
   - Docker daemon running and accessible to Jenkins.
   - SonarQube server properly configured.
2. Add required credentials in Jenkins:
   - GitHub credentials for repository access.
   - Docker Hub credentials for pushing the image.
   - SonarQube authentication token.
3. Trigger the pipeline in Jenkins by selecting the job and clicking `Build Now`.
4. Monitor the pipeline execution to ensure all stages complete successfully.

---
## Outputs

### Jenkins: Successfully automated the pipeline execution.

<img width="959" alt="image" src="https://github.com/user-attachments/assets/37614c11-d975-4358-bba3-eedfba3e5782" />

<img width="958" alt="image" src="https://github.com/user-attachments/assets/791d114c-1671-478e-a1ba-b7379b6113ac" />


### SonarQube: Passed the code quality checks with no major issues.

Url - http://localhost:9000/

<img width="959" alt="image" src="https://github.com/user-attachments/assets/bee6c161-ecbd-4a18-994a-4b4ce639de97" />


### Deployment: The project was deployed successfully.

Url - http://172.28.121.165:8070/login

<img width="953" alt="image" src="https://github.com/user-attachments/assets/2b9c8b46-0277-483c-bc38-b033348f3fdc" />

<img width="959" alt="image" src="https://github.com/user-attachments/assets/226a7c12-a503-49f0-b32e-0672554c75b8" />



This pipeline ensures a streamlined workflow for building, analyzing, and deploying the Ekart application efficiently and reliably.
