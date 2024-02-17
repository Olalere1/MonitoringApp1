__Resource Monitoring App__

**Part 1**: **Deployment of App and dependencies on localhost**

Essentially I was saddled with deploying on AWS cloud a monitoring application scripted in Python that constantly checks on resources - (memory and CPU), used, thus providing efficient costs optimized performance monitoring, automated autoscaling, security intrusion observation and a host of other use cases. The app was first dockerized and ran on local host for the sole purpose of checking that the app codes, requirements/dependencies for the app and desired base image are all appropriate and working fine for the deployment. Then subsequent steps were undertaken on AWS platform. (ref code: app.py, templates - index.html, requirements.txt and Dockerfile)

![image](https://github.com/Olalere1/MonitoringApp1/assets/52172707/2abf2f49-73b7-462d-b74c-830f6e883452)

![image](https://github.com/Olalere1/MonitoringApp1/assets/52172707/56513883-9a20-497a-9b11-1786f3bb1929)


**Part 2**: **Creation and pushing container image to AWS native repository**

The next bit was pushing the docker image into AWS image repository (Elastic Container Repository - ECR). Though the ECR repository could easily have been created directly from the AWS console, but for the purpose of documentation and detailed reference, I opted to use the python boto3 module for the creation. More so, this creates a sort of uniformity in programming language from App up to the final deployment. (ref code: ecr.py)

**Part 3**: **Creation of clusters and nodes**

This stage commenced with the creation of an Elastic Kubernetes Cluster (EKS), somewhat similar to 'minikube cluster' if we had done this on localhost. Once the clusters are created on the AWS console, it has to be authenticated in the CLI interface;

             aws eks update-kubeconfig --namme <EKS cluster name just created>
             
In creating the node group, it is important to ensure that all the necessary policies (SSM GetParameters, EC2 access ...) are attached to the roles, else an unhealthy status will be indicated.

**Part 4**: **Creation of deployment and services for access to the App (ref code: eks.py)**
The image value in the deployment script must tally with the image URi in our ECR registry. Thereafter the script is actived using 
                         python3 <name of deployment script>

The smooth functionality of the deployment and services can be confirmed through the following checks; 
                          kubectl get deployment -n default (check deployments)
                          kubectl get service -n default (check service)
                          kubectl get pods -n default (to check the pods)

If pods and services are confirmed to be working satisfactorily, the port can be exposed for web access using;

                         kubectl port-forward service/<service_name> 5000:5000

