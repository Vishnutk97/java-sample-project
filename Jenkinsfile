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
                        timeout(time: 10,unit: 'MINUTES') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                    catch (Exception ex) {
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
                stage ('Deploy to Dev') {
                    steps {
                        echo 'Build'
                    }
                }

                stage ('Deploy to test') {
                    steps {
                        echo 'Build'
                    }
                }
                stage ('Deploy to Prod') {
                    steps {
                        echo 'Build'
                    }
                }
            }
        }

