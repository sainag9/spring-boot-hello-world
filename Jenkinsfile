node('maven') {
  stage('Build') {
    git url: "https://github.com/sainag9/spring-boot-hello-world.git"
    sh "mvn package"
    stash name:"jar", includes:"target/cart.jar"
  }
  stage('Test') {
    parallel(
      "Tests": {
        sh "mvn test"
      }
    )
  }
  stage('Build Image') {
    unstash name:"jar"
    sh "oc start-build cart --from-file=target/cart.jar --follow"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'cart'
    openshiftVerifyDeployment depCfg: 'cart', replicaCount: 1, verifyReplicaCount: true
  }
}
