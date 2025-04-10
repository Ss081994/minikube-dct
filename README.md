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



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>


