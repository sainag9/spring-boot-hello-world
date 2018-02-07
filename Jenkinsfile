pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven_3_5_2') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven_3_5_2') {
                    sh 'mvn test'
                }
            }
        }
        
        stage ('Deployment Stage') {

            steps {
                openshiftDeploy depCfg: 'cart'
                openshiftVerifyDeployment depCfg: 'cart', replicaCount: 1, verifyReplicaCount: true
                }
            }
        }
    }
}
