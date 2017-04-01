```groovy

def JENKINS_BUILD_NUMBER = System.getenv("BUILD_NUMBER") ?: "1"

version = (JENKINS_BUILD_NUMBER != "1") ? "0.0.${JENKINS_BUILD_NUMBER}" : "${version}"

jar.doFirst{
   logger.lifecycle path + " Jenkins Build Number: ${version}"
}

task updateVersion {
  isCI();

  if(!isRelease()){
    // def jenkinsBuildNumber = System.getProperty("BUILD_NUMBER") ?: "1"
    def jenkinsBuildNumber =  "1"
    if(project.hasProperty("BUILD_NUMBER")){
      jenkinsBuildNumber = project.property("BUILD_NUMBER")
    }

    logger.lifecycle path + " Jenkins Build Number: ${jenkinsBuildNumber}"
    project.version = "0.0.${jenkinsBuildNumber}-SNAPSHOT"
  }
}

boolean isCI() {
    def ciEnvironment = false;
    if (!isRelease()) {
        if (project.hasProperty("CI")) {
            ciEnvironment = project.property("CI")
        }
    }

    logger.lifecycle path + " Are We running in CI environment: ${ciEnvironment}"
    ciEnvironment
}

boolean isRelease() {
    def isRelease = false
    if (project.hasProperty("release")) {
        isRelease = project.property("release")
    }
    isRelease
}

jar.dependsOn(updateVersion)



uploadArchives {
  logger.lifecycle path + " Project Version: ${project.version}"
    repositories{
      mavenDeployer {
        def deployUrl = "${nexusUrl}/repository/"
        def isSnapShot = project.version.contains("SNAPSHOT")
        def deploymentUrl = deployUrl += (isSnapShot ? "maven-snapshots/" :  "maven-releases/" )
        repository(url: deploymentUrl){
         authentication(userName: "${nexusUser}", password: "${nexusPassword}")
       }

        pom.groupId = group
        pom.artifactId = jar.baseName
        pom.version = version
      }
    }
}

artifacts {

}

```