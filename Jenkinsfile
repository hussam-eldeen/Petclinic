pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/jaiswaladi246/Petclinic.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }
        
        
        
         stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
        
       
        
        stage("Deploy To Tomcat"){
            steps{
                script {
                    // Copy the WAR file to the Tomcat container
                    docker.image('tomcat')
                          .withRun("-p 8888:8080 --name quirky_blackwell ")
                          .inside {
                       sudo docker.cp('/home/hussam/Downloads/Petclinic-main.war', 'tomcat-container:/usr/local/tomcat/webapps')
                    }
                 }
                }
        }
                                   
              //  sh "cp  /var/lib/jenkins/workspace/CI-CD/target/petclinic.war /opt/apache-tomcat-9.0.65/webapps/ "
           // }
       // }
        
       
    }
}
