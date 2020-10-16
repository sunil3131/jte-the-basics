pipeline {
  
  agent any


  parameters {
        string(name: 'repositoryBranch', defaultValue: 'master', description: 'repositoryBranch')
        string(name: 'repositoryCredential', defaultValue: 'github', description: 'repositoryCredential')
        string(name: 'repositoryUrl', defaultValue: '', description: 'repositoryUrl')
        string(name: 'registry', defaultValue: 'sunil5252/demoproject', description: 'registry')
        string(name: 'registryCredential', defaultValue: 'dockerhub', description: 'registryCredential')

      string(name: 'manifestRepository', defaultValue: '', description: 'manifestRepository')
      string(name: ' manifestCredential', defaultValue: 'gitlab', description: ' manifestCredential')


}

  stages {
    stage('Clone Repository') {
      steps {
        deleteDir()
        checkout([
          $class: 'GitSCM',
          branches: [[name: "*/${params.repositoryBranch}"]],
          doGenerateSubmoduleConfigurations: false,
          extensions: [],
          submoduleCfg: [],
          userRemoteConfigs: [[
            name: 'origin',
            credentialsId: "${params.repositoryCredential}",
            url: "${params.repositoryUrl}"
          ]]
        ])
      }
    }

    stage('Build Image') {
      steps {
        sh "docker build --network host -t ${params.registry}:${BUILD_NUMBER} ."
      }
    }

    

    stage('Deployment') {
      steps {
        echo "Deployment COMPLETED"
      }

      post {
        always {
          echo "Deployment COMPLETED"
          deleteDir()
          sh "docker rmi ${params.registry}:${BUILD_NUMBER}"
        }
        failure {
          echo "Deployment of ${params.registry}:${BUILD_NUMBER} FAILED"
        }
        success {
          echo "Deployment of ${params.registry}:${BUILD_NUMBER} SUCCEEDED"
        }
      }
    }
  }
}
