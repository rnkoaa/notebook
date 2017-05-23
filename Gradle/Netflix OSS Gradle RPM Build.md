# Netflix OSS tasks for generating build artifacts

```groovy
buildscript {

  dependencies {
    classpath "com.netflix.nebula:gradle-ospackage-plugin:${osPackageVersion}"
  }
}

apply plugin: 'nebula.ospackage'

ospackage {
  packageName = '${project.name}'
  // Uses the main project version
  version = "${project.version}"

  release = System.getenv("BUILD_NUMBER") ?: '0'

  os = LINUX
  type = BINARY
  arch = NOARCH

  // Sets our working directory and permissions, basically
  into "/opt/local/${project.name}"

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
  directory("/opt/local/${project.name}/logs", 0755)

  /* Creates a symlink to the jar file as an init.d script
     (this functionality is provided by Spring Boot) */
  link("/etc/init.d/${project.name}",
    "/opt/local/${project.name}/bin/${project.name}.jar")

  /* According to Spring Boot, the conf file needs to sit
     next to the jar, so we just create a symlink */
  link("/opt/local/${project.name}/bin/${project.name}.conf",
    "/opt/local/${project.name}/conf/${project.name}.conf")
}

```