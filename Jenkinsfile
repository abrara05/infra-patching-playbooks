pipeline {
  agent any

  environment {
    GIT_REPO = 'https://github.com/abrar05/infra-patching-playbooks.git'
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
        ansiblePlaybook credentialsId: 'ansible-key',
                         inventory: 'inventory/test.ini',
                         playbook: 'playbooks/patch-windows.yml'
      }
    }
  }

  post {
    success {
      mail to: 'abrara05@gmail.com',
           subject: "Jenkins Build Successful",
           body: "The pipeline executed successfully."
    }
    failure {
      mail to: 'abrara05@gmail.com',
           subject: "Jenkins Build Failed",
           body: "Check the Jenkins console for errors."
    }
  }
}
