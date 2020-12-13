def commit_id
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
                sh "docker run -d -p 8081:8080 app-nodejs:${commit_id}"
                echo 'deployment complete'
                
            }
        }
    }
}
