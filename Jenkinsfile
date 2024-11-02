pipeline {
    agent any
    
    stages {
        
        stage("code") {
            steps {
                // Checkout the code from Git repository
                git url: "https://github.com/Mazin2k2/node-todo-app-pipelined.git", branch: "master" 
                echo '....Code Cloned....'
            }
        }
        
        stage("build and test") {
            steps {
                // Build the Docker image
                sh "docker build -t node-app-test-new ."
                echo 'code built'
            }
        }
        
        stage("scan image") {
            steps {
                echo 'image scanning done'
            }
        }
        
        stage("push") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    // Login to Docker Hub
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    // Tag the image
                    sh "docker tag node-app-test-new:latest ${dockerHubUser}/node-app-test-new:latest"
                    // Push the image to Docker Hub
                    sh "docker push ${dockerHubUser}/node-app-test-new:latest"
                    echo 'image pushed'
                }
            }
        }
        
        stage("deploy") {
            steps {
                // Use full path for docker-compose if necessary
                sh "/usr/local/bin/docker-compose down"
                sh "/usr/local/bin/docker-compose up -d"
                echo 'deployment done'
            }
        }
    }
}
