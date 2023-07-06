pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }
        
        stage('SonarQube analysis') {
            when {
                anyOf {
                    branch 'master'
                }
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    try {
                        timeout(time: 10, unit: 'MINUTES') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                    catch (Exception ex) {
                        // Handle the exception if needed
                    }
                }
            }
        }
        
        
        stage('Push') {
            steps {
                echo 'Push'
            }
        }

        stage('Deploy') {
            parallel {
                stage('Deploy to Dev') {
                    steps {
                        echo 'Deploy to Dev'
                        // Add deployment steps for Dev environment
                    }
                }

                stage('Deploy to Test') {
                    steps {
                        echo 'Deploy to Test'
                        // Add deployment steps for Test environment
                    }
                }

                stage('Deploy to Prod') {
                    steps {
                        echo 'Deploy to Prod'
                        // Add deployment steps for Prod environment
                    }
                }
            }
        }
    }
}
