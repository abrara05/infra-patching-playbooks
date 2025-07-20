pipeline {
  agent any

  environment {
    GIT_REPO = 'https://github.com/your-username/infra-patching-playbooks.git'
    BRANCH = 'main'
  }

  stages {
    stage('Clone Repo') {
      steps {
        git branch: "${BRANCH}", url: "${GIT_REPO}"
      }
    }

    stage('Lint Playbooks') {
      steps {
        sh 'ansible-lint playbooks/*.yml'
      }
    }

    stage('Run Molecule Tests') {
      steps {
        sh 'molecule test'
      }
    }

    stage('Deploy to Test Servers') {
      steps {
        ansiblePlaybook credentialsId: 'your-ansible-ssh-key-id',
                         inventory: 'inventory/test.ini',
                         playbook: 'playbooks/patch-windows.yml'
      }
    }
  }

  post {
    success {
      mail to: 'your-email@example.com',
           subject: "Jenkins Build Successful",
           body: "The pipeline executed successfully."
    }
    failure {
      mail to: 'your-email@example.com',
           subject: "Jenkins Build Failed",
           body: "Check the Jenkins console for errors."
    }
  }
}
