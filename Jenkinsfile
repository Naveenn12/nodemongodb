pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh 'ssh naveenn@192.168.1.114 docker version'
	sh 'ssh naveenn@192.168.1.114 docker-compose version'
	sh 'ssh naveenn@192.168.1.114 curl --version' 
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'ssh naveenn@192.168.1.114 docker system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
	sh 'echo "#bin/bash \n export job_name=$JOB_NAME" > /tmp/properties.sh'
	sh 'scp /tmp/properties.sh naveenn@192.168.1.114:/tmp/.'
        sh 'ssh naveenn@192.168.1.114 chmod 755 /tmp/properties.sh'
        sh 'ssh naveenn@192.168.1.114 source /tmp/properties.sh'
	sh 'ssh naveenn@192.168.1.114 echo $job_name'
	sh 'ssh naveenn@192.168.1.114 cd /home/naveenn/jenkins_home/workspace/$job_name/'
	sh 'ssh naveenn@192.168.1.114 docker-compose up --force-recreate -d --no-color'
        sh 'ssh naveenn@192.168.1.114 docker ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'ssh naveenn@192.168.1.114 curl http://localhost:80'
      }
    }
  }
  post {
    always {
      sh 'ssh naveenn@192.168.1.114 docker-compose down --remove-orphans -v'
      sh 'ssh naveenn@192.168.1.114 docker compose ps'
    }
  }
}
