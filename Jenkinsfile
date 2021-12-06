pipeline {
   environment {
    registry = "arunkumarniit/azure-vote-front"
    registryCredential = "dockerhub_credentials"
    dockerImage =''
   }
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            powershell(script: 'docker images -a')
            powershell(script: """
               cd azure-vote/
               docker images -a
               docker build -t ${registry} .
               docker images -a
               cd ..            
            """)
         }
      }
       stage('Start test app') {
         steps {
            powershell(script: """
               docker-compose up -d
               ./scripts/test_container.ps1
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            powershell(script: """
               py ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            powershell(script: """
               docker-compose down
            """)
         }
      }
      stage('Push Container') {
         steps {
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote")
            {
               script {
                  docker.withRegistry("https://index.docker.io/v1/", registryCredential)
                  {
                     def image = docker.build(registry) 
                     image.push(); 
                  } 
               }
            }
         }
      }
      // stage('Container Scanning')
      // {
      //    parallel {
      //       stage('Run Anchore') {
      //          steps {
      //             sleep(time: 10, unit: 'SECONDS')
      //             powershell(script: """
      //                Write-Output: ${registry} 
      //             """)
      //          }
      //       }
      //       stage('Run Trivy') {
      //          steps {
      //             powershell(script: """
      //                   docker pull aquasec/trivy:0.21.1 
      //                """)
      //             powershell(script: """
      //                   docker run --rm -v C:/root/.cache/ aquasec/trivy:0.21.1 ${registry}
      //                """)
      //          }
      //       }
      //    }
      // }
      stage('Deploy to QA') {
         environment {
            ENVIRONMENT ='qa'
         }
         steps {
            echo "Deploying to ${ENVIRONMENT}"
         }
      }
      stage('Approve PROD Deploy') {
         when {
            branch 'main'
         }
         options {
            timeout(time:1, unit:'HOURS')
         }
         steps {
            input message: "Deploy ?"
         }
         post {
            success {
               echo "Production Deploy Approved"
            }
            aborted {
               echo "Production Deploy Denied"
            }
         }
      }
      stage('Deploy to PROD') {
         when {
            branch 'main'
         }
         environment {
            ENVIRONMENT = 'prod'
         }
         steps {
            echo "Deploying to ${ENVIRONMENT}"
         }
      }

   } 
}
