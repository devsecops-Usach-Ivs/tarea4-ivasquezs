pipeline {
    agent any
    stages {  
      stage('SCM') {
         steps {
            figlet 'SCM'
            checkout scm // clonacion de codigo en nodo
         }
      }
   
      stage('BUILD') {
         steps {
            figlet 'BUILD'
            sh 'set +x; chmod 777 gradlew'
            sh './gradlew clean build'
         }
      }
      stage('SAST') {
         steps {
            withCredentials([string(credentialsId: 'SONARTOKEN', variable: 'SONARTOKEN')]) {
                 figlet 'SAST'
                 sh('set +x; ./gradlew sonarqube -Dsonar.login=$SONARTOKEN -Dsonar.branch.name=feature-jenkins-ivs')
            }
         }
      }
      stage('SCA') {
         steps {
               dependencyCheck additionalArguments: ''' 
                  -o "./" 
                  -s "./"
                  -f "ALL" 
                  --prettyPrint''', odcInstallation: 'OWASP-Dependency-Check'
               dependencyCheckPublisher pattern: 'dependency-check-report.xml'
               sh 'mv dependency-check-report.xml /tmp/jenkins/reports/$JOB_NAME-$BUILD_ID-' 
         }
      } 
   }
}
