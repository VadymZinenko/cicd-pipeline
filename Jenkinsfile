pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        script {
          git(url: 'https://github.com/VadymZinenko/cicd-pipeline.git', branch: 'main')
        }
      }
    }

    stage('Application Build') {
      steps {
        sh 'chmod +x scripts/build.sh && scripts/build.sh'
      }
    }

    stage('Tests') {
      steps {
        sh 'chmod +x scripts/test.sh && scripts/test.sh'
      }
    }

    stage('Docker Image Build') {
      steps {
        script {
          sh 'docker build -t vadymzinenko/msdp-cicd .'
        }
      }
    }

    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-id') {
            docker.image("vadymzinenko/msdp-cicd:${env.BUILD_ID}").push('latest')
            docker.image("vadymzinenko/msdp-cicd:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }
      }
    }
  }
}
