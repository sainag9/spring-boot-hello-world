node('maven') {
  // define commands
  def mvnCmd = "mvn"
  // injection of environment variables is not done so set them here...
  def sourceRef = "master"
  def sourceUrl = "https://github.com/sainag9/spring-boot-hello-world"
  def devProject = "ocp-tasks-6"
  def applicationName = "jkf-tasks"

  stage 'build'
    git branch: sourceRef, url: sourceUrl
    sh "${mvnCmd} clean install -DskipTests=true"
  stage 'test'
    sh "${mvnCmd} test"
  stage 'deployInDev'
    sh "rm -rf oc-build && mkdir -p oc-build/deployments"
  echo "target"
    sh "cp target/spring-boot-hello-world-1.0-SNAPSHOT.war oc-build/deployments/ROOT.war"
    // clean up. keep the image stream
    sh "oc project ${devProject}"
    sh "oc delete bc,dc,svc,route -l application=${applicationName} -n ${devProject}"
    // create build. override the exit code since it complains about existing imagestream
    sh "oc new-build --name=${applicationName} --image-stream=jboss-eap70-openshift --binary=true --labels=application=${applicationName} -n ${devProject} || true"
    // build image
    sh "oc start-build ${applicationName}-docker --from-dir=oc-build --wait=true -n ${devProject}"
    // deploy image
    sh "oc new-app ${applicationName}:latest -n ${devProject}"
    sh "oc expose svc/${applicationName} -n ${devProject}"
}
