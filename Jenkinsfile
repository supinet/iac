pipeline {
  agent {
    docker {
      image 'node:22-dind'
      args '-u root --privileged -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  // environment {
  //   REGISTRY = 'ghcr.io/your-org-or-user' // replace with your actual registry
  //   BACKEND_IMAGE = "${env.REGISTRY}/your-backend"
  //   FRONTEND_IMAGE = "${env.REGISTRY}/your-frontend"
  // }

  options {
    skipStagesAfterUnstable()
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Backend') {
      steps {
        dir('backend') {
          sh 'npm ci'
          sh 'npm run build'
        }
      }
    }

    stage('Test Backend') {
      steps {
        dir('backend') {
          sh 'npm run test'
        }
      }
    }

    stage('Build Frontend') {
      steps {
        dir('frontend') {
          sh 'npm ci'
          sh 'npm run build'
        }
      }
    }

    stage('Test Frontend') {
      steps {
        dir('frontend') {
          sh 'npm run test'
        }
      }
    }

    stage('Docker Build & Push') {
      when {
        expression { env.GIT_TAG_NAME?.startsWith("dev-") || env.GIT_TAG_NAME?.startsWith("qa-") || env.GIT_TAG_NAME?.startsWith("prod-") }
      }
      steps {
        script {
          def tag = env.GIT_TAG_NAME.replaceAll('/', '-')
          dir('backend') {
            sh "docker build -t ${env.BACKEND_IMAGE}:${tag} ."
            sh "docker push ${env.BACKEND_IMAGE}:${tag}"
          }
          dir('frontend') {
            sh "docker build -t ${env.FRONTEND_IMAGE}:${tag} ."
            sh "docker push ${env.FRONTEND_IMAGE}:${tag}"
          }
        }
      }
    }

    stage('Deploy') {
      when {
        expression { env.GIT_TAG_NAME?.startsWith("dev-") || env.GIT_TAG_NAME?.startsWith("qa-") || env.GIT_TAG_NAME?.startsWith("prod-") }
      }
      steps {
        script {
          def envTarget = ''
          if (env.GIT_TAG_NAME.startsWith("dev-")) envTarget = 'development'
          else if (env.GIT_TAG_NAME.startsWith("qa-")) envTarget = 'qa'
          else if (env.GIT_TAG_NAME.startsWith("prod-")) envTarget = 'production'

          echo "Deploying to ${envTarget} environment..."
          // Replace this with actual deployment script
          sh "./scripts/deploy.sh ${envTarget} ${env.GIT_TAG_NAME}"
        }
      }
    }
  }

  post {
    always {
      echo 'Pipeline finished.'
    }
    success {
      echo 'Build and tests succeeded.'
    }
    failure {
      echo 'Build or tests failed.'
    }
  }
}
