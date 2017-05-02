# Example script to build rpm artifacts and deploy it using jenkins

```sh
# execute the following gradle tasks
./gradlew clean build buildRpm

# Then run the following commands to deploy the rpm as a service
sudo service application-name stop || true
sudo rpm -e application-name || true

sudo rpm -Uvh build/distributions/application-name-1.0-${BUILD_NUMBER}.noarch.rpm

sudo service application-name start
```