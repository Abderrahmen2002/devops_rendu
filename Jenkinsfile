pipeline {

 agent any

 tools {maven 'M2_HOME'}

 stages {

 stage('GIT') {

           steps {

               git branch: 'master',

               url: ' https://github.com/hwafa/timesheetproject.git'

          }

     }

 stage ('Build') {

 steps {

 sh 'mvn clean compile'

 }

 }

 }

}
