pipeline {
    agent any
    tools{
        java 'java'
        maven 'mvn'
    }
    
    environment{
        SCANNER_HOME= tool 'sonarqube'
    }
    
    stages {
        stage('Git Chekout') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/chaudharysurya14/jPetStore_CICD_Project.git'
            }
        }
        
        stage('Generate and compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Generate and compile') {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage('Check Dependency') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        
        
        stage('Static Analysis') {
            steps {
                sh '''$SCANNER_HOME/bin/sonarqube -Dsonar.url=http://192.168.6.131:9000/ -Dsonar.login=sonarqube credential token Dsonar.projectName=JPetStore
                        Dsonar.java.binaries=. \
                        -Dsonar.projectkey=JPetStore '''
            }
        }
        // stage('Generate and build') {
        //     steps {
        //         sh "mvn clean install"
        //     }
        // }
    }
}
