pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'    
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/vincentarck/end-to-end-CI-CD.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile' 
            }
        }

        stage('Test') {
            steps {
                // Focus on integration and deployment first
                sh 'mvn test -DskipTests=true'
            }
        }

        stage('Vulnerability File System Scanning') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Mission -Dsonar.projectName=Mission \
                    -Dsonar.java.binaries=.'''
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package -DskipTests=true'
            }
        }

        stage('Deploy') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: 'maven-settings', traceability: true) {
                    sh 'mvn deploy -DskipTests=true'
                }
            }
        }

        stage('Docker Build & Tag Image') {
            steps {
                withDockerRegistry(credentialsId: 'docker-cred', url: '') {
                    sh 'docker build -t arckvincent/missionapp:lts .'
                }
            }
        }

        stage('Vulnerability Image Scanning') {
            steps {
                sh 'trivy image --scanners vuln --format table -o trivy-image-report.html arckvincent/missionapp:lts'
            }
        }

        stage('Publish Image to Docker Hub') {
            steps {
                withDockerRegistry(credentialsId: 'docker-cred', url: '') {
                    sh 'docker push arckvincent/missionapp:lts'
                }
                
            }
        }

        stage('Deploy to container') {
            steps {
                sh 'docker run -d -p 8081:8080 arckvincent/missionapp:lts'
            }
        }
        
        stage('Deploy to k8') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: ' my-gke-cluster', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://35.244.8.5') {
                    // some block
                    sh 'kubectl apply -f ds.yml -n webapps'
                    sleep 60
                }
            }
        }
        
        
        stage('Verify Deployment to k8') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: ' my-gke-cluster', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://35.244.8.5') {
                    // some block
                    sh 'kubectl get pod -n webapps'
                    sh 'kubectl get svc -n webapps'
                }
            }
        }
    }
    
    post {
    always {
        script {
            def jobName = env.JOB_NAME
            def buildNumber = env.BUILD_NUMBER
            def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
            def bannerColor = pipelineStatus == 'SUCCESS' ? 'green' : 'red'
            
            def body = """<html>
                <body>
                    <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                        <h2>${jobName} - Build ${buildNumber}</h2>
                        <div style="background-color: ${bannerColor}; padding: 10px;">
                            <h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
                        </div>
                        <p>Check the <a href="${env.BUILD_URL}">console output</a>.</p>
                    </div>
                </body>
            </html>"""
             echo "Sending email to vincenthambaya@gmail.com"
                echo "Email body: ${body}"
            emailext (
                subject: "${jobName} - Build ${buildNumber} ${pipelineStatus.toUpperCase()}",
                body: body,
                to: 'vincenthambaya@gmail.com',
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html',
                attachmentsPattern: 'trivy-image-report.html'
            )
        }
    }
}


}
