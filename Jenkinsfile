pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven_3_5_2'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    } 
  }
}
