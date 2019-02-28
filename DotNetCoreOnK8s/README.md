# DotNetCoreOnK8s

Just a simple ASP.NET Core Web Api project that allows me to deploy and
test a .NET application in Kubernetes

## Usage

### Build the Docker image
```
# Build image with latest tag
make build

# Add tag to Docker image
make build tag=test
```

### Push the Docker image
```
make push
```

### Apply the Kubernetes manifest
```
make apply
```