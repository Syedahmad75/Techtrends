# Cloud Native Fundamentals: TechTrends

### Introduction
echTrends is an online website used as a news sharing platform, that enables consumers to access the latest news within the cloud-native ecosystem. In addition to accessing the available articles, readers are able to create new media articles and share them.
we have to package and deploy TechTrends to kubernetes using CI/CD pipeline.The web application is written using the Python Flask framework. It uses SQLite, a lightweight disk-based database to store the submitted articles.
Project Steps Overview: 
1. Apply the best development practices and develop the status and health check endpoints for the TechTrends application.
2. Package the TechTrends application by creating a Dockerfile and Docker image.
3. Implement the Continuous Integration practices, by using GitHub Actions to automate the build and push of the Docker image to DockerHub.
4. Construct the Kubernetes declarative manifests to deploy TechTrends to a sandbox namespace within a Kubernetes cluster. The cluster should be provisioned using k3s in a vagrant box.
5. Template the Kubernetes manifests using a Helm chart and provide the input configuration files for staging and production environments.
6. Implement the Continuous Delivery practices, by deploying the TechTrends application to staging and production environments using ArgoCD and the Helm chart.

### Folder Structure
1. argocd - the folder that will contain the ArgoCD manifests
2. helm - the folder that will contain the Helm chart files
3. kubernetes - the folder that will contain Kubernetes declarative manifests
4. Vagrantfile - the file containing the configuration for the vagrant box. Will be used to create a vagrant box locally.
5. docker_commands - the file will be used to record any used Docker commands and outputs
6. screenshots - the folder that will contain all the screenshots that we take throughout the project.
### Prerequisite

1. Install Python using the instructions provided in the [official Python documentation](https://www.python.org/downloads/) 

2. Install Git using the instructions provided in the [official Git documentation](https://git-scm.com/downloads) 

3. Install Docker using the instructions provided in the [official Docker documentation](https://docs.docker.com/get-docker/)

4. Install Vagrant using the instructions provided in the [official Vagrant documentation](https://www.vagrantup.com/downloads) 

5. Install VirtualBox using the instructions provided in the [official VirtualBox documentation](https://www.virtualbox.org/wiki/Downloads). Ensure that you have VrtualBox `6.1.16` or higher installed.
6. Fork the (project repository)[https://github.com/Syedahmad75/Techtrends]


### Steps
1. To run this application follow these steps:
Initialize the database by using the python init_db.py command. This creates or overwrites (if the file already exists) the database.db file that is used to store and access the available posts.
Run the TechTrends application by using the python app.py command. The application is running on port 3111 and you can access it by querying the http://127.0.0.1:3111/ endpoint.
2. Best Practices For Application Deployment
    1. Healthcheck endpoint
        Build the `/healthz` endpoint for the TechTrends application. The endpoint should return the following response:
        An HTTP 200 status code
        A JSON response containing the `result: OK - healthy message`
    2. Metrics endpoint
        Build a `/metrics` endpoint that would return the following:
        An HTTP 200 status code
        A JSON response with the following metrics:
        Total amount of posts in the database
        Total amount of connections to the database. For example, accessing an article will query the database, hence will count as a connection.
        Example output: {"db_connection_count": 1, "post_count": 7}
    3. Logs
        Extend the TechTrends application to log the following events:
        An existing article is retrieved. The title of the article should be recorded in the log line.
        A non-existing article is accessed and a 404 page is returned.
        The "About Us" page is retrieved.
        A new article is created. The title of the new article should be recorded in the logline.
        Every log line should include the timestamp and be outputted to the STDOUT. Also, capture any Python logs at the DEBUG level.
3. Docker for Application Packaging
    1. This step focuses on packaging the application using Docker. You will write a Dockerfile and build a Docker image for the TechTrends project. By the end of this step, you should have the application running locally inside a Docker container.
    2. Using the Dockerfile defined above, create a Docker image and test it locally. Use the Docker Commands in the `docker_commands` file. 
    3. Run and test locally
    4. Access the application in the browser using the http://127.0.0.1:7111 endpoint and try to click on some of the available posts, create a new post, access the metrics endpoint, etc
    5. Also, the logs can contain custom application logs when an event occurs e.g. create a new post or access an article, and Python logs, such as record when GET requests are performed, etc.
4. Continuous Integration with GitHub Actions
    1. This step aims to use the Continuous Integration (CI) fundamentals and automate the packaging of the TechTrends application. You will use GitHub Actions to build, tag, and push the TechTrends Docker image to DockerHub. As a result, you should have a functional GitHub Action that will construct a new image with every new commit to the main branch.
    2. Create a GitHub Action that will package and push the new image for the TechTrends application to DockerHub. The configuration file should have the name techtrends-dockerhub.yml in the .github/workflows/ directory. If the directory does not exist, create it using the mkdir -p .github/workflows/ command.
    3. These functionalities should be implemented using the Build and Push Docker images upstream GitHub Action as the basis. The following action uses DockerHub Tokens and encrypted GitHub secrets to login into DockerHub and to push new images. To set up these credentials refer to the following resources:
    -Create (DockerHub Tokens)[https://www.docker.com/blog/docker-hub-new-personal-access-tokens/]
    -Create (GitHub encrypted secrets)[https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets]
    4. After creating the GitHub Action verify it executes successfully when a new commit is pushed to the master branch. Verify your DockerHub account for the TechTrends image with the tag latest being pushed successfully. Use the `.github/workflows/techtrends-dockerhub.yml` file to create a Github actions workflow. 
    5. ![Github Tokens](https://i.imgur.com/7rQwNVT.png)
    6. ![CI-GithubActions](https://i.imgur.com/16fhpfV.png)
    7. ![DockerHub](https://i.imgur.com/DtvYpaB.png)
    8. ![DockerHub](https://i.imgur.com/Es5qW0q.png)
5. Kubernetes Declarative Manifests
    1. In this step, you will deploy a Kubernetes cluster using k3s and deploy the TechTrends application. You will create declarative Kubernetes manifests and release the application to the sandbox environment. By the end of this step, you should have a collection of YAML manifests that will manage the TechTrends application within the cluster.
    2. Deploy a Kubernetes cluster
    3. Using vagrant, create a Kubernetes cluster with k3s. Refer to the Vagrantfile from the course repository. Make sure to have vagrant and VirtualBox 6.1.16 or higher installed.
    4. To create a vagrant box and ssh into it, use the following commands:
    5. ```
        # create a vagrant box using the Vagrantfile in the current directory
        vagrant up

        # SSH into the vagrant box
        # Note: this command uses the .vagrant folder to identify the details of the vagrant box
        vagrant ssh
        ```
    6. To deploy the Kubernetes cluster, refer to the k3s documentation. Note: To interact with the cluster kubectl, you need to have root access to the kubeconfig file. Hence, use `sudo su -` to become root and use kubectl commands.
    7. Verify if the cluster is operational by evaluating if the node in the cluster is up and running. You can use the `kubectl get no` command.
    8.  ![Kubernetes](https://i.imgur.com/Es5qW0q.png)
    9.  Deploy TechTrends with Kubernetes manifests using files in the `kubernetes` project folder. 
    10. Using the Kubernetes manifests and kubectl commands, deploy the TechTrends application to the k3s cluster. As a result, you should have the following resource created:
    -a sandbox namespace\
    -a techtrends deployment, in the sandbox namespace with 1 replica or pod running\
    -a techtrends service that exposes the TechTrends application on port 4111 using a ClusterIP\
    11. ```
        # Assuming you are in the /Syedahmad75/Techtrends/tree/main/kubernetes directory
        cd /Syedahmad75/Techtrends/tree/main/kubernetes
        # Transfer the files from local host to the Vagrant VM
        vagrant scp kubernetes /home/vagrant
        # Or you can create your own Yaml file without copying it from local host. Just use 
        vi deploy.yaml 
        # Copy the deploy.yaml content and save it. 
        # i is for insert, :w is for save and :q is for exit. :wq is for save and exit. 
        # Now create thr resources 
        kubectl apply -f kubernetes/
        # Alternatively, you can run these commands oone after another as
        kubectl apply -f deploy.yaml
        kubectl apply -f service.yaml
        kubectl apply -f namespace.yaml
        # To inspect all the resources within the namespace, use the following command:
        kubectl get all -n "namespace name"
        ```
    12.   ![Kubernetes](https://i.imgur.com/1uF5Jk4.png)
6. Helm Charts
   1. Throughout this step, you will use a template configuration manager, such as Helm, to parameterized the TechTrends manifests. You will build a Helm Chart to template and release the application to multiple environments. As a result, you should have a collection of parametrized YAML manifests that will use an input values file to generate valid Kubernetes objects.
   2. The Helm chart is defined in the Chart.yaml file, which contains the API version, name and version of the chart.
   3. Values are under `values.yaml` which represents the default parameters of application deployment if it is not overwritten by a different values file.
   4. in the `values-prod` and `values-staging` file we have the values will override the default parameters:
8. Continuous Delivery with ArgoCD
    1. In this final step, you will deploy the TechTrends automatically using Continuous Delivery fundamentals. You will make use of ArgoCD to release the application to staging and production environments using the templated manifests from the Helm chart. By the end of this step, you should have an automated and templated procedure to deploy TechTends to multiple environments.
    2. Deploy ArgoCD
    3. https://youtu.be/TJrSM31Jj_8
    4. ![ArgoCD](https://i.imgur.com/LWq6XIU.png)
    5. ArgoCD Applications
    6. Create ArgoCD Applications resources to deploy TechTrends to staging and production environments. You need to reference the TechTrends Helm chart built in the previous step and use the respective input files. As such, create the following ArgoCD application manifests.
    7. Using kubectl commands apply the ArgoCD Applications manifests. Make sure to synchronize the application in ArgoCD, so that all the TechTrends resources is deployed successfully. As a result, you should have 2 new namespaces, staging and prod, a deployment for each environment (each with a different amount of pods), and a service exposing the application on different ports.
    8. Copy the `helm-techtrends-prod.yaml` file content and create a new yaml file inside the vagrant using vi command and place the content there. Same for `helm-techtrends-staging`. 
    9. ![ArgoCD](https://i.imgur.com/X6ovM0s.png) 
    10. Now apply the kubectl apply command to create the application. 
    11. `kubectl apply -f helm-techtrends-prod.yaml`
    12. Synch the Application
    13. ![ArgoCD](https://i.imgur.com/7awKLmw.png) 
    14. ![ArgoCD](https://i.imgur.com/9dT91ga.png)
    15. validate it using the command `kubectl get ns`
    16. ![ArgoCD](https://i.imgur.com/tvmUvri.png)
    17. validate it using the command `kubectl get all -n "namespace name"`
    18. ![ArgoCD](https://i.imgur.com/plFeTuX.png)
