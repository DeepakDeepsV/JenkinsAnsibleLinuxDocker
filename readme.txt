Creating Jenkins Server with Maven build
	
	Launch Linux machine
	Install Java (jre) and also its jdk
	include the Java folder path (/usr/lib/) to .bash_profile path as JAVA_HOME
	download and unzip maven and place the file in /opt folder
	add maven folder /opt/apache_maven as MAVEN_HOME and /opt/apache_maven/bin as M2
	add both MAVEN_HOME and M2 to path
	Restart the linux server once

	Install Jenkins using following steps
		1)sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo
		2)sudo rpm --import http://pkg.jenkins.io/redhat/jenkins.io.key
		3)amazon-linux-extras install epel -y 
		4)sudo yum install jenkins
		5)sudo service jenkins start/stop/restart
		6)sudo chkconfig jenkins on
	Configuring Jenkins App
		1) make sure to add java_home and maven_home to Jenkins path in the application
		2) add the git repository as source
		3) add build steps as maven
		4) execute "Test Install" to get the webapp.war file in target folder 

Publish Over SSH
	
	Create a new Application server where the war file will be deployed
	Give password to the user using Passwd ec2-user as sudo su -
	Enable password authentication on the file using vi /etc/ssh/sshd_config. (This is to make sure that the app server accepts credentials from jenkins)
	Reload Service with service sshd reload
	Add and configure publish over ssh in jenkins with the server ip, username and password of the app server. test the connection
	add a post build step in your project and publish over ssh to the app server. you can check the path of the source file in workspace.

Docker Installation on App server
	
	Install docker with "yum install docker" as root
	cat /etc/group  - to see all groups available for root user
	giving permission access to group with command "usermod -aG docker ec2-user"
	work as ec2-user
	start docker service "sudo service docker start"
	create a file in the name "Dockerfile" and paste the below commands.
		FROM tomcat:latest
		WORKDIR /usr/local/tomcat/
		COPY webapp.war webapps
		RUN cp -r webapps.dist/* webapps
	build a new docker image from the docker file using "docker build -t <imagename> ."

SSH configuration to connect to other app servers
	
	1) generate keys in primary server using ssh-keygen
	2) share the keys with destination using ssh-copy-id <user>@targetipaddress
	3) share the keys with your localhost as well
	3) make sure that the destination servers as well as the localhost has password authentication set to enabled 

Ansible Installation and configuration
	1) Install python and then ansible usin pip
	2) create hosts file
	3) create playbook.yml
	4) place the command "ansible-playbook -i hosts playbook.yml" in jenkins exec command and test build once

Jenkins-Git Continuous integration
	1) create build trigger for github scm
	2) make sure to create webhook with the jenkins url in the github settings
	3) make changes to the repo and check if it is reflecting. 
	
	
	

	






