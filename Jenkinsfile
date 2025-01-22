pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                script {
                    withEnv(["DOCKER_HOST=tcp://localhost:2375"]) {
                        sh "docker stop container || true"
                        sh "docker rm container || true"
                        def dockerImage = 'localhost:5000/jenkins-image'
                        docker.image(dockerImage).pull()
                        docker.image(dockerImage).run("--user root --rm -it -v ${pwd()}:/mnt --name container")
                    }
                }
            }
        }
        stage('Run Script') {
            steps {
                script {
                    sh "docker exec container /bin/bash -c 'python3 /mnt/sample.py'"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
            script {
                sh "docker stop container || true"
                sh "docker rm container || true"
            }
        }
    }
}
