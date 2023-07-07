pipeline {
    agent any
    
    stages {

        // CI Start
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }

        stage("SonarQube analysis") {
            agent any
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
        
        stage("Quality Gate") {
            steps {
              timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
                
        stage('Push') {
            steps {
                echo 'Push'
            }
        }

        // Ci Ended

        // CD Started

       stage ('Deployments') {
           parallel {
               
        stage('Deploy to Dev') {
            steps {
                echo 'Build'
            }
        }

        stage('Deploy to test ') {
            when {
                branch 'master'
            }
            steps {
                echo 'Build'
            }
        }
            
    }
        // CD Ended
}
    }
}
