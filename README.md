### Built With

This section should list any major frameworks/libraries used to bootstrap your project. Leave any add-ons/plugins for the acknowledgements section. Here are a few examples.

* Minikube
* Docker
* YAML


<!-- GETTING STARTED -->
## Getting Started
To get a local copy up and running follow these simple example steps.

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.
* minikube
* brew(if MacOS)
* docker
  
### Installation

_Below is an example of how you can instruct your audience on installing and setting up your app. This template doesn't rely on any external dependencies or services._


1. Install minikube cluster
   ```sh
   minikube start --cpus=4 --memory=8192
   ```
2. Check the status that the control plane is up
   ```sh
   minikube status 
   ```
3. Enable the Ingress Controller 
   ```sh
   minikube addons enable ingress
   ```
4. Deploying DCT on minikube
   ```sh
   helm repo add dct-services https://dlpx-helm-dct.s3.amazonaws.com
   ```
5. Pull the appropriate helm chart to deploy 
    ```sh
   helm pull dct-services/delphix-dct --version 2025.1.2
   ```
5. Extract the chart
    ```sh
   tar -xvf delphix-dct-2025.1.2.tgz
   ```


### Configure the Chart values and deploy
1. Modify the chart values.yaml to include these two changes  
    ```sh
    apiKeyCreate: true
    Provide the Docker registry credentials (username and password) for pulling images.
   ```
2. Deploy the chart
   ```sh
    helm install dct-services delphix-dct
   ```

### Ingress route creation

  Provided you use minikube tunnel to access the dct console we need to expose the proxy pod HTTP port. (port 80)

  1. Edit the values.yaml
   ```sh
    useSSL: false
   ```
 2. Run the upgrade
   ```sh
   helm upgrade dct-services -f values.yaml delphix-dct
   ```

3. Create the TLS certificate 
   ```sh
   openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout server.key \
  -out server.crt \
  -subj "/CN=local.dct/O=SelfSigned"
    ```
   ```sh
    kubectl create secret tls ingress-tls \
  --namespace dct-services \
  --key server.key \
  --cert server.crt
 ```
   
4. Update the /etc/hosts file to update the hostname 


<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>


