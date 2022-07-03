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
          //archiveArtifacts artifacts: "build/libs/testing-web-*.jar"
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
   }
}
