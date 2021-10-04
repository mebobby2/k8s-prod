import java.text.SimpleDateFormat

pipeline {
  options {
    buildDiscarder logRotator(numToKeepStr: '5')
    disableConcurrentBuilds()
  }
  agent {
    kubernetes {
      cloud "kubernetes"
      label "prod"
      serviceAccount "build"
      yamlFile "KubernetesPod.yaml"
    }
  }
  environment {
    cmAddr = "cm.192.168.99.104.nip.io"
  }
  stages {
    stage("deploy") {
      when {
        branch "main"
      }
      steps {
        container("helm") {
          sh "helm repo add chartmuseum http://${cmAddr}"
          sh "helm repo add stable https://charts.helm.sh/stable"
          sh "helm repo add jenkins https://charts.jenkins.io"
          sh "helm repo update"
          sh "helm dependency update"
          sh "helm upgrade -i prod --namespace prod --force"
        }
      }
    }
    stage("test") {
      when {
        branch "main"
      }
      steps {
        echo "Testing..."
      }
      post {
        failure {
          container("helm") {
            sh "helm rollback prod 0"
          }
        }
      }
    }
  }
}
