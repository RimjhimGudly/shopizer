pipeline {
  agent any
  stages{
    stage ('Build') {
      steps{
        echo "Building Project"
        sh 'mvn install -DskipTests=true'
      }
    }
    stage ('Archive') {
      steps{
        echo "Archiving Project"
        archiveArtifacts artifacts: '**/*.war', followSymlinks: false
      }
    }
   stage('Build images') {
	      steps {
		sh '''
			  cd sm-shop
			  sudo docker build -f "Dockerfile" -t rimjhimdockerhub/shopizer-app:latest . 
		'''
      }
    }
   stage('Push Image'){
	       steps{
	         withDockerRegistry([credentialsId: 'DOCKER_HUB_CREDENTIALS', url: '']) {
   			sh 'docker push rimjhimdockerhub/shopizer-app:latest'
	}
      }
    }
   stage('Deploy to dev env') {
	      steps {
		sh '''
			  docker rm -f shopizer || true
			 docker run -d --name=shopizer -p 8081:8080 rimjhimdockerhub/shopizer-app:latest  
		'''
      }
    }
  }
}
