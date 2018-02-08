node('maven') {
  // define commands
  def mvnCmd = "mvn"
  // injection of environment variables is not done so set them here...
  def sourceRef = "master"
  def sourceUrl = "https://github.com/sainag9/spring-boot-hello-world"
  def devProject = "ocp-tasks-8"
  def applicationName = "spring-boot-hello-world"

  stage 'build'
    git branch: sourceRef, url: sourceUrl
    sh "${mvnCmd} clean install -DskipTests=true"
  stage 'test'
    sh "${mvnCmd} test"
  stage 'sonar'
    node {
              withSonarQubeEnv('My SonarQube Server') {
                 sh 'mvn clean package sonar:sonar'
              }
          }
}
