pipeline {
  agent any
  stages {
    stage('Verify branch') {
      steps {
        bat(script: """
          echo ${GIT_BRANCH}
          echo ${GIT_URL}
          echo ${BUILD_NUMBER}
          echo ${NODE_NAME}
          echo ${JOB_NAME}
          echo ${WORKSPACE}
        """) 
      }
    }
    stage('Docker Build') {
      steps {
        sh(script: 'docker images -a')
        sh(script: """
          cd azure-vote/
          docker build -t jenkins-pipeline .
          docker images -a
          cd ..
        """)
      }
    }
    stage('Start test app') {
      steps {
        sh(script: """
          ./scripts/test_container.sh
        """)
      }
      post {
        success {
          echo "App started successfully"
        }
        failure {
          echo "App failed to start"
        }
      }
    }
    stage('Run tests') {
      steps {
        sh(script: """
          pytest ./tests/test_sample.py
        """)
      }
    }
  }
}