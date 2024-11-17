pipeline {
    agent {
        label "lab-server"
    }

    environment {
        SONAR_HOST_URL = "http://192.168.189.103:9000"
        SONAR_TOKEN = "sqa_107ef127e21d5b8639ae623e9f4f40f514a2de00"
    }

    stages {
        stage('Checkout Code Backend') {
            steps {
                git branch: 'main', url: 'https://github.com/v4224/Backend.git'
            }
        }

        stage('SonarQube Analysis - Begin') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube-server') {
                        sh """
                            dotnet sonarscanner begin \
                            /k:"my-backend-project" \
                            /d:sonar.host.url=$SONAR_HOST_URL \
                            /d:sonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t onlineshop-backend ."
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                script {
                    sh(script: """docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy clean --all""")
                    sh(script: """docker run --rm -v \$(pwd):/backend -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --format template --template "@contrib/html.tpl" --output /backend/backend-trivy-scan.html onlineshop-backend""")
                }
            }
        }

        // stage('Snyk Scan') {
        //     steps {
        //         script {
        //             sh(script: """docker build --rm --network host --build-arg SNYK_AUTH_TOKEN=$SNYK_TOKEN --build-arg OUTPUT_FILENAME=snyk-scan -t snyk-scan -f Dockerfile-snyk .""")
        //             sh(script: """docker create --name snyk-scan snyk-scan""")
        //             sh(script: """docker cp snyk-scan:/app/snyk-scan.html .""")
        //         }
        //     }
        // }

        stage('Deploy Backend to Docker') {
            steps {
                script {
                    sh """
                        docker rm -f onlineshop-backend || true
                        docker run --name onlineshop-backend -dp 5214:5214 onlineshop-backend
                    """
                }
            }
        }
    }
    post {
        always {
            //archiveArtifacts artifacts: 'report-task.txt', fingerprint: true
            archiveArtifacts artifacts: 'backend-trivy-scan.html', fingerprint: true
            //archiveArtifacts artifacts: 'backend-snyk-scan.html', fingerprint: true
        }
    }
}
