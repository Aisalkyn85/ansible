pipeline {
  agent any

  environment {
    // Define Python virtual environment path
    VENV_DIR = "${WORKSPACE}/venv"
  }

  stages {
    stage('Checkout Repository') {
      steps {
        // Pull latest code from your GitHub repo
        git branch: 'main', url: 'https://github.com/Aisalkyn85/ansible.git'
      }
    }

    stage('Set Up Python Environment') {
      steps {
        sh '''
        # Install Python if not already available
        which python3 || brew install python

        # Create virtual environment if missing
        if [ ! -d "$VENV_DIR" ]; then
          python3 -m venv $VENV_DIR
        fi

        # Activate venv and install Ansible
        source $VENV_DIR/bin/activate
        pip install --upgrade pip
        pip install ansible
        ansible --version
        '''
      }
    }

    stage('Install Required Collections') {
      steps {
        sh '''
        source $VENV_DIR/bin/activate
        ansible-galaxy collection install community.general --force
        '''
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        script {
          // You can replace playbook.yml with any playbook name
          sh '''
          source $VENV_DIR/bin/activate
          ansible-playbook playbook.yml
          '''
        }
      }
    }
  }

  post {
    always {
      echo "Pipeline finished. Cleaning up workspace..."
      cleanWs()
    }
  }
}
