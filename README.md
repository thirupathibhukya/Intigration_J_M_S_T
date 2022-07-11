# Intigration_J_M_S_T

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
JENKINS PIPELINE ( JENKINS + MAVEN + GIT HUB + SONAR + TOMCAT )
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Note: Install ssh-agent plugin and generate code using pipeline syntax

pipeline{
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/thirupathibukya/maven-web-app.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('Sonar-Server-7.8') {
                    sh "mvn sonar:sonar"
                }
            }
        }
		stage('Code deploy') {
            steps{
				sshagent(['Tomcat-Server-Agent']) {
				  sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@13.235.68.29:/home/ec2-user/apache-tomcat-9.0.63/webapps'
				}
            }
        }
    }
}
