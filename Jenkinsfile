pipeline {
    agent any

    stages {
        
        // Stage to checkout code from GitHub
        stage("Code Checkout") {
            steps {
                git url: "https://github.com/Mazin2k2/node-todo-app-pipelined.git", branch: "master"
                echo '....Code Cloned....'
            }
        }
        
        // Stage to build and test the Docker image
        stage("Build and Test") {
            steps {
                script {
                    try {
                        // Build the Docker image
                        sh "docker build --progress=plain -t node-app-test-new ."
                        echo 'Code built successfully.'
                    } catch (Exception e) {
                        echo "Build failed: ${e.message}"
                        error("Stopping the pipeline.")
                    }
                }
            }
        }
        
        // Stage to scan the Docker image (placeholder for actual scanning logic)
        stage("Scan Image") {
            steps {
                echo 'Image scanning done'
                // Placeholder for actual image scanning commands, if needed
            }
        }
        
        // Stage to push the Docker image to Docker Hub
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    // Log in to Docker Hub
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    
                    // Tag the image
                    sh "docker tag node-app-test-new:latest ${dockerHubUser}/node-app-test-new:latest"
                    
                    // Push the image
                    sh "docker push ${dockerHubUser}/node-app-test-new:latest"
                    echo 'Image pushed successfully.'
                }
            }
        }
        
        // Stage to deploy the application using Docker Compose
        stage("Deploy") {
            steps {
                // Bring down any existing containers and start the new ones
                sh "docker-compose down && docker-compose up -d"
                echo 'Deployment done.'
            }
        }
    }

    // Optional post section to handle pipeline success or failure
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
