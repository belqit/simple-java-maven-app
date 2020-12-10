pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        // stage('Publish on Nexus as latest') {
        //     // If the build not failed in any stage (including the sub-builds triggered in the Functional tests) the latest war will be deployed on nexus.
        //     when {
        //         expression { currentBuild.currentResult == 'SUCCESS' }
        //     }
        //     steps {
        //         dir('development') {
        //             script {
        //                 sh 'mvn -B deploy -DskipTests -Dpmd.skip=true'
        //                 sh 'mvn -B deploy -DskipTests -Dpmd.skip=true -Dapp.version=latest'
        //             }
        //         }
        //     }
        // }
    }
}