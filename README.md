# CourseProj1660Final
Walkthrough of Course Project\
Here are the steps I used in order to complete the Course Project\
1.Get the Docker Containers\
> I built the main application by altering the example application used in https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app. \
2. Tested other containers for hadoop, sonarqube/sonarscanner, spark, and jupyter.\
3. For all four images pulled, tagged, then pushed to gcr\
    a. Example Commands for Jupyter Image: docker pull jupyter\
    b. docker tag jupyter gcr.io/cs1660proj/jupyter\
    c. docker push gcr.io/cs1660proj/jupyter\
4. Created my cluster named my-cluster, set to autopilot \
    a. Command: gcloud container clusters create-auto my-cluster
6. Deployed containers in GCR to my-cluster.\
    a. For hadoop namenodes and datanodes I had to add environment variables that were within this file: https://github.com/big-data-europe/docker-hadoop/blob/master/hadoop.env\
    b. Example Command for Jupyter: kubectl create deployment jupyter --image=jupyter/datascience-notebook
6. Edit yaml files so they use 250m cpu and 1Gi memory so that I had enough for all\
7. Expose the pods.\
    a. Jupyter was exposed to port 8888 \
    b. Sonarqube: 9000 \
    c. Main application: 8080 \
    d. Spark: 8080 \
    e. Hadoop: 9870:9000 \
    f. Example Command for Jupyter: kubectl expose deployment jupyter --name=jupyter-service --type=LoadBalancer --port 80 --target-port 8888
    *For hadoop I manually created the service so I could expose both ports.
8. Update the Main Terminal Application so that it displays the links for all the pods \
    Commands: kubectl get svc (to see the external ip addresses) \
    docker build -t us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 . \
    docker push us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 \
    kubectl set image deployment/hello-app hello-app=us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 (This line was executed after I changed print statements to have the external IP addresses) \

How to Use Application:
1. Navigate to 34.75.201.101
2. Follow Instructions
