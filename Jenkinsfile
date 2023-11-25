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
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/hussam-eldeen/Petclinic.git'
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
            steps {
                     deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://10.20.13.130:8888/')], contextPath: null, war: '**/*.war'
                }
            }
       
                                   
              //  sh "cp  /var/lib/jenkins/workspace/CI-CD/target/petclinic.war /opt/apache-tomcat-9.0.65/webapps/ "
           // }
       // }
        
       
    }
}
