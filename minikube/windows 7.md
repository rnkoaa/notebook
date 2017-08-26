# Notes concerning minikube

To start a minikube instance on windows, use powershell and run the following commands.

```sh
minikube start --kubernetes-version="v1.6.1" \
--vm-driver="virtualbox" --show-libmachine-logs \
--alsologtostderr
```