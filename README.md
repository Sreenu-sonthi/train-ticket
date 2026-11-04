Train-Ticket-Booking Application — Multi-Environment CI/CD Deployment
1. Project Overview
The Train-Ticket-Booking Application is a Java-based web application packaged as a WAR file and deployed to Apache Tomcat. This project demonstrates an automated Continuous Integration and Continuous Deployment (CI/CD) process using GitHub Actions, Maven, Git, Apache Tomcat, and AWS EC2.
The application is deployed independently to four environments:
•	DEV — Development and initial validation
•	TEST — Functional and integration testing
•	PRE-PROD — Final validation before production
•	PROD — Live production environment
Each environment has its own deployment target and configuration. When code is pushed to a configured branch, the corresponding GitHub Actions workflow builds the application and deploys the WAR file to that environment.
Project-specific details: The project has been discussed with Maven WAR packaging, the artifact name Train-Ticket-Booking-application.war, Tomcat 9, and the context path /Train-ticket-Booking-application. Confirm the paths, branch names, and server details in your actual setup before using deployment commands.
________________________________________
2. Project Objectives
•	Build the Gym Application using Maven.
•	Automate CI/CD using GitHub Actions.
•	Deploy the application independently to DEV, TEST, PRE-PROD, and PROD.
•	Use branch-based deployment workflows.
•	Package the application as a WAR file.
•	Deploy the WAR to Apache Tomcat.
•	Store environment-specific credentials securely.
•	Reduce manual build and deployment work.
•	Keep testing and production deployments separated.
•	Support controlled production releases.
3. Technologies Used
Technology	Purpose
Java 17	Application development and runtime/build compatibility, as configured
Maven	Build automation and dependency management
Apache Tomcat 9	Java web application server
Git	Version control
GitHub	Source code hosting
GitHub Actions	CI/CD automation
AWS EC2	Cloud server infrastructure
Ubuntu Linux	Server operating system, if used on EC2
Shell scripting	Deployment automation
GitHub Environments	Environment-specific secrets and deployment approvals
4. Project Architecture
CI/CD deployment flow
Developer
   |
   v
GitHub Repository
   |
   v
Environment Branch
   |
   v
GitHub Actions Workflow
   |
   +--> Checkout source code
   |
   +--> Set up Java
   |
   +--> Maven clean verify
   |
   +--> Verify Train-Ticket-Booking-application.war
   |
   +--> Deploy to the selected environment
   |
   v
Environment-specific EC2 instance / Tomcat server
   |
   v
 Train-Ticket-Booking Application
   |
   v
Browser
Environment separation
Each environment should have its own deployment target and environment-specific settings.
                     GitHub Repository
                            |
                      GitHub Actions
                            |
          +-----------------+-----------------+
          |                 |                 |
         DEV               TEST            PRE-PROD
          |                 |                 |
       Tomcat             Tomcat            Tomcat
          |                 |                 |
          +-----------------+-----------------+
                            |
                           PROD
                            |
                          Tomcat
The diagram represents logical separation. Your actual infrastructure may use separate EC2 instances, separate Tomcat services, or another environment-isolation design.
5. Multi-Environment Deployment Strategy
The application is deployed independently to four environments.
Environment	Purpose
DEV	Development and initial validation
TEST	Functional testing and integration testing
PRE-PROD	Production-like validation before release
PROD	Live application deployment
Example branch-to-environment mapping
Use the branch names that exist in your repository. The following is an example mapping:
Git branch	GitHub Environment	Purpose
Dev	DEV	Development and initial validation
Test	TEST	Application testing
Pre-Prod	PRE-PROD	Final pre-production validation
main	PROD	Production deployment
Branch names are case-sensitive. If your repository uses names such as Develop, stagging, or pre-prod, update the workflow triggers to match them exactly.
Example deployment flow
Dev branch       --> DEV
Test branch      --> TEST
Pre-Prod branch  --> PRE-PROD
main branch      --> PROD
Promote changes through the environments only after the required validation is completed. Protect the production branch and use GitHub Environment approval rules for production releases.
6. Repository Structure
A typical Maven web application repository may use a structure similar to this:
Train-Ticket-Booking-application/
├── .github/
│   └── workflows/
│       ├── dev.yml
│       ├── test.yml
│       ├── pre-prod.yml
│       └── prod.yml
├── src/
│   └── main/
│       ├── java/
│       ├── resources/
│       └── webapp/
│           ├── WEB-INF/
│           │   └── web.xml
│           └── index.jsp
├── pom.xml
└── README.md
The exact source folders depend on the application's implementation. The .github/workflows/ folder contains the workflow definitions, and target/ is generated by Maven during a build.
7. CI/CD Pipeline Explanation
Stage 1: Source Code Management
Developers maintain the Gym Application source code in GitHub. Git is used to track changes and manage branches.
Stage 2: Workflow Trigger
A push to a configured branch triggers the matching workflow. A workflow may also support manual execution using workflow_dispatch.
Stage 3: Checkout Source Code
GitHub Actions checks out the repository:
- name: Checkout Source Code
  uses: actions/checkout@v4
Stage 4: Set up Java
The workflow configures Java 17 and Maven dependency caching:
- name: Set up Java 17
  uses: actions/setup-java@v4
  with:
    distribution: temurin
    java-version: '17'
    cache: maven
If the Maven pom.xml is in a subdirectory, configure cache-dependency-path to point to that file.
Stage 5: Maven Build
Maven builds and verifies the project:
mvn -B clean verify
This command removes previous build output and runs the Maven lifecycle through verification. It creates the WAR file when the project is configured for WAR packaging.
Stage 6: WAR File Verification
Verify that the expected artifact was generated:
ls -lh target/
test -f target/Train-Ticket-Booking-application.war
Expected artifact:
target/Train-Ticket-Booking-application.war
Stage 7: Deployment
The WAR file is deployed to the Tomcat server for the selected environment. Deployment may use the Tomcat Manager Text API or an SSH/SCP-based deployment process, depending on the configured workflow.
For a Tomcat Manager deployment, the deployment account requires the manager-script role, and the Manager application may restrict requests by source IP. A 403 response means the request is not authorized; check the account roles and Manager access restrictions.
Stage 8: Deployment Verification
Verify the deployed application using the environment-specific URL:
http://<ENVIRONMENT_TOMCAT_HOST>:8080/Train-Ticket-Booking-application/
Replace the placeholder with the actual host for DEV, TEST, PRE-PROD, or PROD.
8. Maven Commands
Command	Purpose
mvn clean	Removes previous build output
mvn compile	Compiles Java source code
mvn test	Runs configured tests
mvn package	Packages the application
mvn verify	Runs verification lifecycle checks
mvn -B clean verify	Performs a clean build and verification in batch mode
Maven artifact configuration
The project should be configured for WAR packaging and the expected final name in pom.xml:
<packaging>war</packaging>

<build>
    <finalName>Train-Ticket-Booking-application</finalName>
</build>
This produces:
target/Train-Ticket-Booking-application.war
9. GitHub Actions Workflows
A separate workflow can be maintained for each environment.
Workflow file	Trigger branch	GitHub Environment
dev.yml	Dev	DEV
test.yml	Test	TEST
pre-prod.yml	Pre-Prod	PRE-PROD
prod.yml	main	PROD
Each workflow typically performs the following tasks:
1.	Check out the source code.
2.	Set up Java.
3.	Build the project using Maven.
4.	Verify the generated WAR.
5.	Deploy to the corresponding Tomcat server.
6.	Perform a deployment check.
7.	Report the workflow result.
The branch names in this table are examples. Ensure the workflow's on.push.branches values match your actual branch names.
10. GitHub Environment Configuration
Create the four environments in:
GitHub Repository → Settings → Environments
Create:
•	DEV
•	TEST
•	PRE-PROD
•	PROD
Configure environment-specific secrets separately. For example, if using Tomcat Manager:
Secret	Description
TOMCAT_HOST	URL of the environment's Tomcat server
TOMCAT_USER	Tomcat deployment username
TOMCAT_PASSWORD	Tomcat deployment password
If deploying through SSH/SCP, use the secrets required by that workflow, such as:
Secret	Description
EC2_HOST	Environment-specific EC2 host or IP
EC2_USER	SSH login user
EC2_SSH_KEY	Private SSH key used by the workflow
Use only the secrets needed by your selected deployment method. Do not hardcode credentials in workflow files.
Production approvals
The PROD GitHub Environment can be configured with required reviewers and deployment protection rules. This allows a production deployment to wait for approval before proceeding.
11. Tomcat Configuration
Apache Tomcat 9 hosts the Train-Ticket-Booking- Application.
The application context path is:
/Train-Ticket-Booking-application
The deployment artifact is:
Train-Ticket-Booking-application.war
For a Tomcat Manager Text API deployment, the configured deployment user needs the manager-script role. Example role/user configuration syntax:
<role rolename="manager-script"/>
<user username="deploy"
      password="CHANGE_THIS_PASSWORD"
      roles="manager-script"/>
This is an example only. Configure credentials securely and restrict access to the Tomcat Manager application to trusted networks. Avoid exposing the Manager API to the public internet.
12. AWS EC2 Deployment
For each environment, ensure the relevant EC2 instance or Tomcat service is configured and reachable by the deployment workflow.
Deployment checklist
1.	Provision or identify the EC2 instance for the environment.
2.	Install the required Java runtime.
3.	Install and configure Apache Tomcat 9.
4.	Confirm the Tomcat service is running.
5.	Configure the environment's deployment credentials.
6.	Configure network access only for required sources and ports.
7.	Deploy the WAR file.
8.	Verify the application URL and Tomcat logs.
Useful server checks
Check whether Tomcat is listening on port 8080:
sudo ss -lntp | grep 8080
Check the Tomcat process:
ps -ef | grep '[t]omcat'
Test the application locally on the server:
curl -I http://localhost:8080/Train-Ticket-Booking-application/
Review Tomcat logs. In the previously discussed setup, Tomcat was installed at /root/tomcat:
tail -n 100 /root/tomcat/logs/catalina.out
If your environment uses another Tomcat path or a systemd service, update these commands accordingly.
13. Application URLs
Use the hostnames or IP addresses configured for each environment:
Environment	Application URL
DEV	http://<DEV_HOST>:8080/Train-Ticket-Booking-application/
TEST	http://<TEST_HOST>:8080/Train-Ticket-Booking-application/
PRE-PROD	http://<PREPROD_HOST>:8080/Train-Ticket-Booking-application/
PROD	http://<PROD_HOST>:8080/Train-Ticket-Booking-application/
Replace the placeholders with the actual environment addresses. For production, use the HTTPS URL provided by your production ingress, reverse proxy, or load balancer when configured.
14. Git Commands
Clone the repository
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_DIRECTORY>
View branches
git branch
git branch -r
git branch -a
Switch branches
git switch Dev
git switch Test
git switch Pre-Prod
git switch main
Use the exact branch names present in your repository.
Add and commit changes
git add .
git commit -m "Update Train-Ticket-Booking Application"
Push changes
git push origin Dev
Push to the branch associated with the intended environment. For example, pushing to the configured DEV branch should trigger the DEV workflow.
View commit history
git log --oneline
15. Troubleshooting
Tomcat returns 404
Possible causes:
•	The WAR was not deployed.
•	The context path is incorrect.
•	The WAR filename does not match the expected artifact.
•	The application failed to start.
Check the deployed files and logs:
ls -lah <TOMCAT_HOME>/webapps/
tail -n 100 <TOMCAT_HOME>/logs/catalina.out
Confirm that the application URL ends with /Train-Ticket-Booking-application/.
Tomcat Manager returns 403
Possible causes:
•	The deployment user lacks the manager-script role.
•	The Manager application blocks the runner's IP address.
•	Remote access is restricted by Tomcat Manager configuration.
Check the Manager user roles and access restrictions. Do not solve this by opening Manager access to everyone.
Maven build fails
Run the build locally:
mvn -B clean verify
Review the first relevant Maven error and check the JDK version, dependencies, and pom.xml configuration.
WAR file is missing
Confirm the project uses WAR packaging and the expected final name. Then rebuild:
mvn clean verify
ls -lh target/
Workflow does not trigger
Check that:
•	The workflow is saved under .github/workflows/.
•	The branch name in the trigger matches the actual branch exactly.
•	GitHub Actions is enabled.
•	The push reached the expected repository and branch.
•	The YAML indentation is valid.
Application fails to start
Review Tomcat logs for deployment or XML configuration errors:
tail -n 150 <TOMCAT_HOME>/logs/catalina.out
If the application uses WEB-INF/web.xml, confirm that it is valid XML and compatible with Tomcat 9.
16. Security Notes
•	Store deployment credentials in GitHub Secrets or environment secrets.
•	Never commit private SSH keys, passwords, or tokens.
•	Apply least-privilege access to EC2 and AWS resources.
•	Restrict SSH access to trusted sources.
•	Restrict Tomcat Manager access to trusted networks.
•	Avoid exposing Tomcat's port directly to the internet for production use.
•	Use HTTPS for production traffic.
•	Configure production approval rules.
•	Maintain deployment logs and a rollback plan.
•	Do not grant broad sudo permissions to a deployment account unless required and carefully restricted.
17. Future Enhancements
Possible improvements include:
•	Automated unit and integration tests.
•	Code quality analysis with SonarQube.
•	Dependency and security scanning.
•	Artifact management using Nexus or JFrog.
•	Deployment health checks.
•	Versioned artifact storage and rollback.
•	Monitoring and centralized logging.
•	HTTPS through a reverse proxy or AWS load balancer.
•	Containerization using Docker.
•	Kubernetes-based deployment, if required by project scope.
18. Author
Sreenivas Sonthi 
GitHub: sreenu-sonthi
Project: Train-Ticket-Booking- Application — Multi-Environment CI/CD Deployment
________________________________________
Project Summary
This project demonstrates an automated build and deployment process for a Java-based Train-Ticket-Booking Application using Maven, GitHub Actions, Apache Tomcat, Git, and AWS EC2. The application is deployed independently across DEV, TEST, PRE-PROD, and PROD, supporting separated validation stages and controlled production releases.
Project Screenshots
1. EC2 Instance
 
2. Screenshot of AMI used for different environment
 
3. EC2 instance for different environments:
 



2. DEV-Environment deployment output:
 
 
2. TEST-Environment deployment output:
 
 
2. PRE-PROD-Environment deployment output:

 
 
2.PROD-Environment deployment output:

 
 
