pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: jenkins-kaniko
spec:
  serviceAccountName: jenkins-sa
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:v1.16.0-debug
      imagePullPolicy: Always
      command:
        - sleep
      args:
        - 99d
"""
    }
  }

  // Тригериться на кожен push у репозиторій. Потребує GitHub webhook,
  // що вказує на <JENKINS_URL>/github-webhook/ (Jenkins GitHub plugin
  // може зареєструвати його автоматично, якщо в Manage Jenkins ->
  // System -> GitHub заданий сервер із github-token credential).
  triggers {
    githubPush()
  }

  environment {
    // TODO: підставити реальний ECR registry (наприклад
    // 123456789012.dkr.ecr.us-west-2.amazonaws.com), створений у infra-репозиторії.
    ECR_REGISTRY = "REPLACE_ME.dkr.ecr.REPLACE_ME.amazonaws.com"
    IMAGE_NAME   = "django-app-ci"
    IMAGE_TAG    = "${env.GIT_COMMIT ?: 'latest'}"
  }

  stages {
    stage('Build & Push Docker Image') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \\
              --context `pwd` \\
              --dockerfile `pwd`/Dockerfile \\
              --destination=$ECR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG \\
              --cache=true \\
              --insecure \\
              --skip-tls-verify
          '''
        }
      }
    }
  }
}
