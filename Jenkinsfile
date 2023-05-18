def projectName = "vcm-ms-web"
def devopsRepository="cicd-demo"
def devOpsrepoVersion="cicd-demo-repo-versions"
def projectGitRepo="https://github.com/narayanahyniva/cicd-demo.git"
def vcmConfiglocalServer="/opt/vcm/vcm-config-server"
def development_s3applicationBucket = "cicd-demoproject"


pipeline {
  agent any

  parameters {
    string(name: 'AKIAVKELNHGDT326B7OJ', defaultValue: '', description: 'AKIAVKELNHGDT326B7OJ')
    string(name: 'Lbj3pySeHAY8Fy26WeR9Cq90FX/CnlWKvmsY3HlR', defaultValue: '', description: 'Lbj3pySeHAY8Fy26WeR9Cq90FX/CnlWKvmsY3HlR')
    string(name: 'cicd-demoproject', defaultValue: '', description: 'cicd-demoproject')
  }

  environment {
    CI = 'true'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Deploy') {
      steps {
        sh 'npm run deploy'
      }
    }

    stage('Upload Artifacts to S3') {
      steps {
        withAWS(credentials: [
          [$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AKIAVKELNHGDT326B7OJ', secretKeyVariable: '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']
        ]) {
          script {
            def artifacts = sh(returnStdout: true, script: 'find build -type f').trim().split('\n')
            for (def artifact in artifacts) {
              def filePath = artifact.substring('build/'.length())
              s3Upload(file: artifact, bucket: "${params.cicd-demoproject}", path: filePath)
            }
          }
        }
      }
    }
  }
}
