pipeline {
	agent any
	 tools{
        // java 'java'
        maven 'maven3'
    }
  	stages {
  stage ('Check secrets') {
    steps {
      sh 'trufflehog3 https://github.com/chaudharysurya14/jPetStore_CICD_Project.git -f json -o truffelhog_output.json || true'
          }
    }
      stage('Check Dependency') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'Owasp-DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
	stage ('Static analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
        //sh 'sudo python3 Devsecops.py'
	}
      }
    }
   stage('Generate and build') {
 steps {
     sh "mvn compile"
            }
        }
//  stage ('Fetch Application server') {
// steps {
// sshagent(['application_server']) {
//       sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/CDAC _Intern_Project/target/webgoat-2023.5-SNAPSHOT.jar root@192.168.80.22:/root/WebGoat'
//       sh 'ssh -o  StrictHostKeyChecking=no root@192.168.80.22 "nohup java -jar /WebGoat/webgoat-server-v8.2.0-SNAPSHOT.jar &"'
//           }
//         }
//       }
   }
}
