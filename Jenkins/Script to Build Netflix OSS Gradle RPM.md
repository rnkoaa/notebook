```sh
# execute the following gradle tasks
./gradlew clean build buildRpm

# Then run the following commands to deploy the rpm as a service
sudo service judicial-migration-service stop || true
sudo rpm -e judicial-migration-service || true

sudo rpm -Uvh build/distributions/judicial-migration-service-1.0-${BUILD_NUMBER}.noarch.rpm

sudo service judicial-migration-service start
```