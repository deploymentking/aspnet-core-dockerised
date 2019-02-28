# DotNetCoreOnK8s

Just a simple ASP.NET Core Web Api project that allows me to deploy and
test a .NET application in Kubernetes

## Usage

### Build the Docker image
```
# Build the image with latest tag
make build

# Build the image with specified tag
make build tag=test
```

### Push the Docker image
```
# Push the image with latest tag
make push

# Push the image with specified tag
make push tag=test
```

### Apply the Kubernetes manifest
```
make apply
```