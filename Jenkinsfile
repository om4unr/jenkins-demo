pipeline {
  agent { 
    node { 
      label 'docker'
    }
  }
  stages {
	stage('Setup') {
          checkout scm
          sh '''
            ./ci/docker-down.sh
            ./ci/docker-up.sh
          '''
        }
	stage('Test'){
        parallel (
            "unit": {
              sh '''
                ./ci/test/unit.sh
              '''
            },
            "functional": {
              sh '''
                ./ci/test/functional.sh
              '''
            }
          )
        }
	stage('Capacity Test') {
          sh '''
            ./ci/test/stress.sh
          '''
        }
    stage('Cleanup') {
          sh '''
            ./ci/docker-down.sh
          '''
        }
  }
}
