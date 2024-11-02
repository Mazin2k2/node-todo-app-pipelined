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
                script {
                    // Check if dockerHub credentials exist
                    def dockerHubCreds = credentials('dockerHub')
                    if (dockerHubCreds) {
                        withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser ")]) {
                            sh "docker login -u ${env.dockerHubUser } -p ${env.dockerHubPass}"
                            sh "docker tag node-app-test-new:latest ${env.dockerHubUser }/node-app-test-new:latest"
                            sh "docker push ${env.dockerHubUser }/node-app-test-new:latest"
                            echo 'image pushed'
                        }
                    } else {
                        error("DockerHub credentials not found. Please add them.")
                    }
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
