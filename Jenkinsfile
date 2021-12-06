@Library('github.com/arunkumarniit/demo-shared-pipeline') _

pythonPipeline {
   repoUrl = "https://github.com/arunkumarniit/myvotingappdemo"
   repoBranch = "shared-library"
   dockerFileFolder = "azure-vote"
   dockerRegistry = "arunkumarniit/azure-vote-front"
   dockerRegistryCredentials = "dockerhub_credentials"
   testsContainerFileLocation = "scripts/test_container.ps1"
   testsScriptsFileLocation = "tests/test_sample.py"
}
 