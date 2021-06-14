# Optional Lab (building docker image from node.js source)

### Build and run node app with docker

As the first step if not already done - clone [this repository](https://github.com/ibm-garage-dach/cloud-native-bootcamp-iks) with:
```
git clone https://github.com/ibm-garage-dach/cloud-native-bootcamp-iks.git
```

And then navigate to the `/node_docker` folder with:

```
cd cloud-native-bootcamp-iks/node_docker
```

### Build a nodejs container image

Use the Dockerfile to build the image:

```bash
docker build -t <your-initals>-node-js-demo:1.0 .
```


### Work with the container image

Run the image:
```bash
docker run --rm -d -p <your-port>:8080 --name <your-initals>-container <your-initals>-node-js-demo:1.0
curl http://localhost:<your-port>
docker ps
```

Run a bash shell in the container and check that `node_modules` were innstalled:
```bash
docker exec -i <your-initals>-container /bin/bash
ls
```

Stop the container:
```bash
docker stop <your-initals>-container
docker ps
```


### Create a namespace in IBM Cloud

Login to IBM Cloud, select your account and target your resource group:
```bash
ibmcloud login --sso                    # follow the instructions
ibmcloud target -g <RESOURCE_GROUP>     # if not default
```

Create a namespace in the IBM Cloud Container Registry:
```bash
ibmcloud cr region-set us-south
```


### Push image

```bash
ibmcloud cr login
docker tag <your-initals>-node-js-demo:1.0 us.icr.io/<bootcamp-namespace>/<your-initals>-node-js-demo:1.0
docker push us.icr.io/<bootcamp-namespace>/<your-initals>-node-js-demo:1.0
ibmcloud cr image-list
docker rmi us.icr.io/<bootcamp-namespace>/<your-initals>-node-js-demo:1.0
docker pull us.icr.io/<bootcamp-namespace>/<your-initals>-node-js-demo:1.0
```
