Below is a simple gradle task that can be used to do a docker build
straight from gradle.

** This sample is taken from Jhipster gradle build tool **

```groovy
import org.gradle.internal.os.OperatingSystem

task buildDocker(type: Exec) {
    doFirst {
        copy {
            from "src/main/docker/"
            into "build/docker/"
            include "*"
        }
        copy {
            from "build/libs"
            into "build/docker/"
            include "*.war*"
        }
    }
    if (OperatingSystem.current().isWindows()) {
        commandLine 'cmd', '/c', 'docker', 'image', 'build', '-f', 'build/docker/Dockerfile', '-t', '${jar.basename}', 'build/docker/'
    } else {
        commandLine 'docker', 'image', 'build', '-f', 'build/docker/Dockerfile', '-t', '${jar.basename}', 'build/docker/'
    }
}


```