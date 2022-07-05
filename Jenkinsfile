pipeline {
    agent any
    environment {
       NOMBRE_REPORTE_SCA = 'dependency-check-report.xml'
       PATH_REPORTES = '/tmp/jenkins/reports/'
    }
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
               figlet 'SCA OWASP-DC'
               dependencyCheck additionalArguments: ''' 
                  -o "./" 
                  -s "./"
                  -f "ALL" 
                  --prettyPrint''', odcInstallation: 'OWASP-Dependency-Check'
               dependencyCheckPublisher pattern: '$NOMBRE_REPORTE_SCA'
               sh 'mv $NOMBRE_REPORTE_SCA $PATH_REPORTES$JOB_NAME-$BUILD_ID-$NOMBRE_REPORTE_SCA'
               sh 'set chmod 777 $PATH_REPORTES$JOB_NAME-$BUILD_ID-$NOMBRE_REPORTE_SCA'
         }
      } 
   }
}
