pipeline {
    agent any
    stages {
	stage('Setup') {
	  steps {
	      script {
		  sh "docker stop container || true"
		  sh "docker rm container || true"
		  def dockerImage = 'localhost:5000/jenkins-image'
		  docker.image(dockerImage).pull()
		  def containerId = docker.image(dockerImage).run("--user root --rm -it -v ${pwd()}:/mnt --name container")
		}
	    }
	}
	stage('Run Script') {
	    steps {
		sh "docker exec container /bin/bash -c 'python3 /mnt/sample.py'"
	    }
	}
      }
      post {
	always {
	   cleanWs()
	   sh "docker stop container || true"
	   sh "docker rm container || true"
	}
    }
}
