pipeline {
  agent any

  environment {
    KUBECONFIG = "${HOME}/.kube/config"
  }

  tools {
    ansible 'Ansible'
  }

  stages {
    stage('Checkout Repository') {
      steps {
        checkout scm
      }
    }

    stage('Pull Docker Images') {
      steps {
        sh 'docker pull ibrahim372/fr:latest'
        sh 'docker pull ibrahim372/bk:latest'
      }
    }

    stage('Deploy with Ansible') {
      steps {
        dir('ansible') {
          sh 'ansible-playbook -i inventory.ini deploy_postgres.yml'
          sh 'ansible-playbook -i inventory.ini deploy_backend.yml'
          sh 'ansible-playbook -i inventory.ini deploy_frontend.yml'
        }
      }
    }
  }

  post {
    success {
      echo "✅ Déploiement terminé avec succès dans le namespace default"
    }
    failure {
      echo "❌ Échec du déploiement"
    }
  }
}
