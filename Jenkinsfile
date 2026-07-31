pipeline {
<<<<<<< HEAD

    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE      = "Jenkins"
        appVersion  = ""
        ACC_ID      = "526426842890"
        PROJECT     = "roboshop"
        COMPONENT   = "catalogue"
        REGION      = "us-east-1"

        ECR_REPO    = "${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}"
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
=======
    agent {
        label 'AGENT-1'
>>>>>>> 6ec7c541ae0ca1aaaff2ee6ab3c54626f9e17809
    }

    stages {

        stage('Checkout') {
            steps {
<<<<<<< HEAD
                checkout scm
            }
        }

        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "Application Version : ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                npm install --include=dev
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                npm test
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {

                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('sonar-qube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=${COMPONENT} \
                        -Dsonar.projectName=${COMPONENT} \
                        -Dsonar.sources=. \
                        -Dsonar.projectVersion=${appVersion}
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh """
                docker build -t ${PROJECT}/${COMPONENT}:${appVersion} .
                """
            }
        }

        // // stage('Trivy Image Scan') {
        // //     steps {
        // //         sh """
        // //         trivy image ${PROJECT}/${COMPONENT}:${appVersion}
        // //         """
        // //     }
        // }

        stage('Login to AWS ECR') {
            steps {
                sh """
                aws ecr get-login-password --region ${REGION} | \
                docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com
                """
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh """
                docker tag ${PROJECT}/${COMPONENT}:${appVersion} ${ECR_REPO}:${appVersion}
                docker tag ${PROJECT}/${COMPONENT}:${appVersion} ${ECR_REPO}:latest
                """
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh """
                docker push ${ECR_REPO}:${appVersion}
                docker push ${ECR_REPO}:latest
                """
=======
                echo 'Checking out source code...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
>>>>>>> 6ec7c541ae0ca1aaaff2ee6ab3c54626f9e17809
            }
        }
    }

    post {
<<<<<<< HEAD

        always {
            echo "Cleaning Workspace..."
            cleanWs()
        }

        success {
            echo "Pipeline Completed Successfully"
        }

        failure {
            echo "Pipeline Failed"
        }

        aborted {
            echo "Pipeline Aborted"
=======
        always {
            echo 'Pipeline execution completed.'
        }

        success {
            echo 'Build completed successfully.'
        }

        failure {
            echo 'Build failed.'
>>>>>>> 6ec7c541ae0ca1aaaff2ee6ab3c54626f9e17809
        }
    }
}