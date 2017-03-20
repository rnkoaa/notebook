```groovy
group='com.group.name'
version='1.0'
description='application description'

def buildTime() {
  final dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ssZ")
  dateFormat.timeZone = TimeZone.getTimeZone('GMT')
  dateFormat.format(new Date())
}

task cleanResources(type: Delete) {
  delete 'build/resources'
}

task cleanWebapp(type: Delete){
 delete 'src/main/resources/static/'
}

```

```groovy
jar {
  baseName = "application-name"
  manifest {
    attributes("Implementation-Title": "${project.description}", "Implementation-Version": "${project.version}")
  }
//  archiveName = "${jar.baseName}.jar"
}

springBoot {

  //This statement tells gradle to make the resulting jar executable
  executable = true

  excludeDevtools = true

  // This statement tells the Gradle Spring Boot plugin
  // to generate a file
  // build/resources/main/META-INF/build-info.properties
  // that is picked up by Spring Boot to display
  // via /info endpoint.
  buildInfo {
    // Generate extra build info.
    additionalProperties = [
      by                   : System.properties['user.name'],
      operatingSystem      : "${System.properties['os.name']} (${System.properties['os.version']})",
      continuousIntegration: System.getenv('CI') ? true : false,
      machine              : InetAddress.localHost.hostName,
      // Override buildInfo property time
      time                 : buildTime(),
      // Override name property
      name                 : 'application-name',
      jenkinsTag           : System.getenv("BUILD_TAG") ?: "",
      jenkinsBuildNumber   : System.getenv("BUILD_NUMBER") ?: ""
    ]
  }
}
```

```groovy
bootRun {
  addResources = true

  systemProperty 'management.info.git.mode', 'FULL'

  //add system properties
  systemProperties System.properties
}

test {
  //add system properties
  systemProperties System.properties
}
```