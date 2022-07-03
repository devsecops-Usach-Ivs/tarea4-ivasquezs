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
   }
}

