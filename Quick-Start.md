# Dependecies
1. docker
2. istioctl
3. kubectl
4. go (for installation, configuration and basic testing, follow instructions in e.g. [How to Install GoLang (Go Programming Language) in Linux](https://www.tecmint.com/install-go-in-linux/); make sure go environment variables and path are configured accordingly)
5. python (for a detailed list of python modules check [model requirements](https://github.com/EricssonResearch/cloud-native-app-simulator/blob/main/model/requirements.txt))
  - flask
  - guinicorn
  
# Build and upload Docker images
Build docker image for main application
1. Under _/model_ directory, run:
``` bash
docker build -t app-demo .
```
2. After creating the docker image, upload it to each of the clusters _i_ by runnning:
``` bash
kind load docker-image app-demo --name={cluster$i}
```

# Environment Preparation

## Deploying a Development Environment
Visit this [Wiki Page](https://github.com/EricssonResearch/cloud-native-app-simulator/wiki/Development-Environment) to learn about development environment setup.

## Generating and Deploying the Application
1. To generate and deploy microservice-based applications, go to the _/generator/_ directory
2. Make sure the _/generator_ directory is located under path _~/go_projects/src/_ and initialize module by executing `go mod init`
3. If needed, install go module dependencies, e.g. cobra and yaml
4. Modify any of the input files under the _input/_ directory according to your own requirements (see some json examples under the _examples/_ directory).
5. Generate and deploy kubernetes manifest files by running the _generator.sh_ and _deploy.sh_ scripts. The generator can be run under two different modes: 
  - **Random mode:** Generates a random description file
  - **Preset mode:** Generates Kubernetes manifest based on a description file in the input directory. Note that this commands generates k8s yaml files which are stored under the _clusterX_ directory(ies) (see some .yaml examples under the _examples/_ directory).
```bash
./generator.sh {mode} {input file}
```  
6. Change Kubernetes context to the main cluster
```bash
kubectl config use-context cluster1
```

## Generating Traffic

Modify the necessary files for traffic generation. For example, with Tsung:

- Change the chain json file under the request section in the _conf.xml_ file to send request to the desired frontend service based on the exposed IP, e.g., for Istio-based deployment, update the http url IP based on the ingress gateway IP where the tsung pod is deployed
- After that, you can just use the following command to start traffic generation
```bash
tsung -f tsung/conf.xml -k start
```
- To stop traffic generation use´
```bash
tsung stop
```

## Traffic Monitoring
You can observe the performance metrics for the application traffic by using the dashboards on the grafana web UI.