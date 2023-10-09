pipeline {
  agent {
    laabel 'docker'
  }
  stages {
    stage("Verifying installation") {
      steps {
        sh ''
        '
        docker version
        docker info
        docker compose version
        curl--version
        jq--version
          ''
        '  
      }
    }

    stage('Pruning docker data') {
      steps {
        sh 'docker system prune -a --volimes -f'
      }
    }

    stage('Starting container') {
      steps {
        sh 'docker compose up -d --no-color --wait'
        sh 'docker compose ps'
      }
    }

    stage('Testing against the container') {
      steps {
        sh 'curl http://localhost:3000/param?query=demo | jq'
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
