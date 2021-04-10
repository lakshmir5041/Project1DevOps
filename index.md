## This project is to demonstrate the continuous integration and delivery by building a Docker Jenkins Pipeline.

```markdown
Tools used in this project

•	Docker: To build the application from a Dockerfile and push it to Docker Hub.
•	Docker Hub: To store the Docker image.
•	GitHub: To store the application code and track its revisions.
•	Git: To connect and push files from the local system to GitHub.

Linux (Ubuntu): As a base operating system to start and execute the project.
Jenkins: To automate the deployment process during continuous integration.

```
The following is a step-by-step procedure from initial installation to final solution with screenshots attached for each step.

1.	Installation of pre-requisites/tools
2.	Jenkins Install and Configuration
3.	Git Install and Configuration
4.	Docker Install and Configuration
5.	Create a GitHub repository and push the source code from local repository to remote repository.
6.	Install Jenkins Plugin “Cloudbees Docker Build and Publish”

Step 1: Installation of pre-requisites/ tools

•	sudo apt update terminal command downloads the package lists from the repositories and "updates" them to get information on the newest versions of packages and their dependencies. 

•	Check if java is already installed.
o	java -version

 


•	If java is not installed in the system, the following command need to be executed in the terminal to successfully install the Java and verify using the above command.
o	sudo apt install -y openjdk-8-jdk

Why is Java required?
•	Java is required because Jenkins is a java-based tool, so it requires Java to run. 

Step 2: Jenkins Install and Configuration

•	Installation of Jenkins.

Add the Jenkins key using the following command:
$wget -q -0- https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add – 

 

•	Add the Jenkins package repo to package manager:
Update the file:
$sudo vi /etc/apt/sources.list
OR
$sudo nano /etc/apt/sources.list

Add this line at the end of the file 
deb https://pkg.jenkins.io/debian-stable binary/

Click Esc and Shift+wq to exit from Edit mode.

 

•	Execute  $sudo apt-get update

•	Install Jenkins $sudo apt-get install -y Jenkins

 

•	Browse: http://localhost:8080

 

•	To get the initial admin password to unlock Jenkins
$sudo cat /var/lib/Jenkins/secrets/initialAdminPassword

 

•	Select the option - Install suggested plugins(recommended)
 

•	Create first admin user
 


 

Commands to manually restart the Jenkins server when necessary.
$sudo systemctl restart jenkins
OR 
$sudo service Jenkins restart
OR 
Browse: http://localhost:8080/restart

Step 3: Git Installation and Configuration

$sudo apt-get update

•	Install Git: $sudo spt-get install -y git

•	If Git is already installed, just verify using the following command

$git –version
 

•	Configure git
 $git config –global user.email “EMAIL_ID”

$git config -global user.name “YOUR_USER_NAME”

 

•	Verify the git configuration $git config –list

 

Step 4: Docker Installation and Configuration

$sudo apt-get update

•	Install Docker: $sudo apr-get install -y docker.io

•	Verify Docker Installation $docker –version

 

•	If the docker installation gives an error(Permission Denied) execute the following commands to grant  necessary permissions for the successful installation.
$sudo docker –version
$sudo usermod -aG docker <your_linux_user_name>
[Relogin]
$docker –version

Step 5: Creating a GitHub Repo and push the source code from local repository to remote repository

•	Create an empty GitHub repository in your GitHub account.
- https://github.com/lakshmir5041/Project1DevOps.git

•	Clone the Git url to Linux machine
$git clone https://github.com/lakshmir5041/Project1DevOps.git 

•	Navigate to the current directory
$cd Project1DevOps

 

•	Create a Dockerfile as below. 

$nano Dockerfile
From java:8
COPY ./var/www/java
WORKDIR /var/www/java
RUN javac Hello.java
CMD [“java”, “Hello”]

Ctrl+X to exit from edit mode and press Y when prompted to save the file

 

•	Create a sample Java file as below.

$nano Hello.java
Class Hello{
	Public static void main(String[] args){
		System.out.println(“This is a java program created using Docker”);
}
}

Ctrl+X to exit from edit mode and press Y when prompted to save the file


 

•	Test the Docker images. Check whether Dockerfile is working without any error and able to build image.

$docker images
$docker build -t test_image


 

$docker images
$docker rmi test_images  //command to remove the image cache.


 

•	Add the files to the local repo, commit the changes & push the changes to remote repo. And make sure the changes are reflected in the remote repo.
$git status 
$git add .
$git status
$git commit -m “Dockerfile created for the new image”
$git status


 


Step 6: Create & Configure Jenkins job. Also build and tracking changes on Jenkins

•	Open Jenkins, create a freestyle project. 
o	Login to Jenkins > New Item > choose custom name for the job > Freestyle project > OK
•	Use the Github url in the Source Code Management. 
o	Source Code Management(tab) > Git > Add the git url in the space provided
 

•	In the build section execute docker build command to test whether Jenkins can execute it.
o	Build(tab) > Add Build step > Execute Shell: docker build -t my_test_image > Save


 

•	Navigate to the Main page and select> Build Now.
•	Click on the current build and select Console Output to check the build messages. 
 
•	If the build fails with an error in console output, and the error being: permission denied
•	Execute the following command on the terminal to grant admin permissions to fix the error.

$sudo usermod -aG docker jenkins

 


•	Restart jenkins  $sudo systemctl restart jenkins

•	Relogin and build the job once again

 
•	To verify docker images $ docker images

 

Step 7: Install Jenkins Plugin “Cloudbess Docker Build & Publish”

•	Steps to Install a Jenkins plugin “Cloudbees Docker Build and Publish”
o	Navigate to Jenkins Dashboard > Select Manage Jenkins from left panel > Select Manage Plugins >Click on Available(tab) > Search for “cloudbees docker” > Select couldbees Docker Build and Publish

 



•	Click on Download now and install after restart
•	Select the check box Restart Jenkins when installation is complete and no jobs are running.

 
•	Jenkins automatically restart if it takes too long, we can manually restart by entering the url in the browser: localhost:8080/restart
•	Login to Jenkins when prompted.

 

•	Open the Jenkins job and click on Configure (option on the left panel).
•	Under Build tab – remove Execute Shell (the command entered earlier).
•	Add Build step-Docker build and Publish.

 

•	Enter the DockerHub Repository name: <docker-id>/myfirstassessmentimg

 
•	Add DockerHub credentials.
o	Click on Add – Enter DockerHub username and password > Click on add.
o	Use this credential as registry credential.

 


•	Save the new Build options build the job once again.

•	To verify the saved docker image
o	Login to the DockerHub account and find the respective image name.

 

•	Check the Build status and messages from the console output of Jenkins latest build.
 

•	(Optional) Steps to Automate
o	Build Trigger
o	Poll – SCM duration *******
