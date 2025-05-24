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

    stage('Deploy with Ansible') {
      steps {
        dir('ansible') {
          sh 'sudo ansible-playbook -i inventory.ini deploy_postgres.yml -vvv'
          sh 'sudo ansible-playbook -i inventory.ini deploy_backend.yml'
          sh 'sudo ansible-playbook -i inventory.ini deploy_frontend.yml'
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
