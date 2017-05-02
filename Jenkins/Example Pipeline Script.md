# Jenkins Piple Script for Building Java Applications

The script below is used to build a java application, deploy it to a maven jar using gradle mavenDeployer, then building a docker image and deploying it to a docker registry.

```groovy
/* Only keep the 10 most recent builds. */
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', numToKeepStr: '5']],
]

/*if (!env.CHANGE_ID) {
    if (env.BRANCH_NAME == null) {
        projectProperties.add(pipelineTriggers([cron('H/30 * * * *')]))
    }
}*/

properties(projectProperties)

try{
  node{
    def applicationName = "YOUR APPLICATION NAME" //example: artifactId
    def dockerImageName = "YOUR DOCKER TAG" // example: group/artifactId
    stage('\u2776 Check out from Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'YOUR GIT REPO'
      credentialsId: 'REPO CREDENTIALS'
    } //check stage complete

    stage('\u2777 Build artifacts') { // for display purposes
      echo "\u2600 BUILD_URL=${env.BUILD_URL}"

      def workspace = pwd()
      // Run the gradle build
      if (isUnix()) {
        sh "chmod +x gradlew"
        sh "./gradlew -PCI=true -PBUILD_NUMBER=${env.BUILD_NUMBER} clean jar"
      } else {
        bat(/gradlew.bat -PCI=true -PBUILD_NUMBER=${env.BUILD_NUMBER} clean jar/)
      }
    } //complete artifact build stage

    stage('\u2778 Deploy Jar to Nexus') { // for display purposes
     if (isUnix()) {
        sh "./gradlew -PCI=true -PBUILD_NUMBER=${env.BUILD_NUMBER} uploadArchives"
      } else {
        bat(/gradlew.bat -PCI=true -PBUILD_NUMBER=${env.BUILD_NUMBER} uploadArchives/)
      }
    } // complete docker build stage

    stage('\u2779 Build docker image') { // for display purposes
      sh "mkdir build/docker && cp src/main/docker/Dockerfile build/docker"
      sh "cp build/libs/*.jar build/docker/${applicationName}.jar"
      dir('build/docker') {
        docker.withRegistry("${env.DOCKER_PRIVATE_REGISTRY_URL}", 'YOUR JENKINS CREDENTIALS ID'){
          def appImage = docker.build("${dockerImageName}:${env.BUILD_NUMBER}")
          appImage.push()
          appImage.push('latest')
        }
      }
    } // complete docker build stage
  }
}catch(exception){
    println exception.message
}


```