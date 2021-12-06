@Library('github.com/arunkumarniit/demo-shared-pipeline') _

pipeline {
   agent any

   stages {
      stage('Call Library Hello-world function') {
         steps {
            script {
               helloWorld()
            }
         }
      }
      stage('Call Library function with an argument') {
         steps {
            script {
               helloArgs('Aruns Jenkins!')
            }
         }
      }
      stage('Call Additional library functions') {
         steps {
            script {
               helloArgs.goodbyeWorld('Jenkins!!')
            }
         }
      }
   } 
}
