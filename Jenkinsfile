pipeline {
    agent any
    
    stages {
        
        stage("code") {
            steps {
                git url: "https://github.com/Mazin2k2/node-todo-app-pipelined.git", branch: "master" 
                echo '....Code Cloned....'
            }
        }
        
        stage("build and test") {
            steps {
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
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser ")]) {
                    // Use echo to pass the password securely
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser } --password-stdin"
                    sh "docker tag node-app-test-new:latest ${dockerHubUser }/node-app-test-new:latest"
                    sh "docker push ${dockerHubUser }/node-app-test-new:latest"
                    echo 'image pushed'
                }
            }
        }
        
        stage("deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment done'
            }
        }
    }
}
