```groovy
buildscript {

  dependencies {
    classpath "com.netflix.nebula:gradle-ospackage-plugin:${osPackageVersion}"
  }
}

apply plugin: 'nebula.ospackage'

ospackage {
  packageName = 'judicial-migration-service'
  // Uses the main project version
  version = "${project.version}"

  release = System.getenv("BUILD_NUMBER") ?: '0'

  os = LINUX
  type = BINARY
  arch = NOARCH

  // Sets our working directory and permissions, basically
  into "/opt/local/judicial-migration-service"

  // Copy the actual .jar file
  from(jar.outputs.files) {
    // Strip the version from the jar filename
    rename { String fileName ->
      fileName.replace("-${project.version}", "")
    }
    fileMode 0500
    into "bin"
  }

  // Copy the config files
  from("tools/deploy/install/unix/conf") {
    fileType CONFIG | NOREPLACE
    fileMode 0754
    into "conf"
  }
}

// Configure our RPM build task
buildRpm {
  //user "jdwms-user"
  //permissionGroup "jdwms-user"

  // Creates an empty log directory
  directory("/opt/local/judicial-migration-service/logs", 0755)

  /* Creates a symlink to the jar file as an init.d script
     (this functionality is provided by Spring Boot) */
  link("/etc/init.d/judicial-migration-service",
    "/opt/local/judicial-migration-service/bin/judicial-migration-service.jar")

  /* According to Spring Boot, the conf file needs to sit
     next to the jar, so we just create a symlink */
  link("/opt/local/judicial-migration-service/bin/judicial-migration-service.conf",
    "/opt/local/judicial-migration-service/conf/judicial-migration-service.conf")
}

```