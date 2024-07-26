pipeline {
    agent {
        label 'Jenkins Agent'
    } 
    environment {
        PATH = "/opt/apache-maven-3.9.4/bin:$PATH"
    }
    stages {
        stage('build'){
            steps {
                echo "----- build started -------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----- build completed -----"
            }
        }
        stage('test'){
            steps{
                echo "----- unit test started -------"
                sh 'mvn surefire-report:report'
                echo "----- unit test completed -------"
            }
        }
        stage('sonarQube analysis'){
            environment{
                scannerHome = tool 'daimyo-sonar-tool'
            }
            steps{
                withOnarQubeEnv(credentialsId: 'sonarqube-key', installationName: 'daimyo-sonarqube-server'){
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        
    }
}