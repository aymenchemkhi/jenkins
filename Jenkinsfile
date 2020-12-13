def commit_id
def container_name="jenkins_pipeline"
pipeline {
    agent any
    

    stages {
        stage('Preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {                        
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }
        stage('Docker image Build') {
            steps {
                echo 'Building....'
                sh "docker build -t app-nodejs:${commit_id} ."
                echo 'build complete'
            }
        }
        stage('Deploy') {
            steps {
                echo'Deploying'
                sh "docker stop  ${container_name}"
                sh "docker run -d --rm -p 8081:8080 --name ${container_name} app-nodejs:${commit_id}"
                echo 'deployment complete'
                
            }
        }
    }
}
