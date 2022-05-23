pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh 'ssh naveenn@192.168.1.114 docker version;docker info;docker-compose version;curl --version;jq --version' 
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'ssh naveenn@192.168.1.114 docker system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
        sh 'ssh naveenn@192.168.1.114 docker compose up -d --no-color --wait'
        sh 'ssh naveenn@192.168.1.114 docker compose ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'curl http://localhost:80'
      }
    }
  }
  post {
    always {
      sh 'docker compose down --remove-orphans -v'
      sh 'docker compose ps'
    }
  }
}
