pipeline {
   environment {
    registry = "arunkumarniit/azure-vote-front"
    registryCredential = 'DockerHub'
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

   } 
}
