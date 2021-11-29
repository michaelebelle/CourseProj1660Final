# CourseProj1660Final
Walkthrough of How I Complete Project Option 1 for 1660 
1. Build Main App
> I built the main application by altering the example application used in https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app. 
> Built this applications container and stored it to gcr with these commands: <br/>
> a. git clone https://github.com/GoogleCloudPlatform/kubernetes-engine-samples <br/>
> b. cd kubernetes-engine-samples/hello-app <br/>
> c. Changed main application to display the messages that I want in the hello.go file <br/>
> d. docker build -t REGION-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v1 . <br/>
2. Tested other containers for hadoop, sonarqube/sonarscanner, spark, and jupyter. 
3. For all images other then main application I pulled, tagged, then pushed to gcr 
> Example Commands for Jupyter Image (Not Used on Main Application): docker pull jupyter/datascience-notebook<br/> 
> docker tag jupyter/datascience-notebook gcr.io/cs1660proj/jupyter<br/>
> docker push gcr.io/cs1660proj/jupyter<br/>
4. Created my cluster named my-cluster, set to autopilot 
    a. Command: gcloud container clusters create-auto my-cluster
5. Set the project ID
    a. Command: export PROJECT_ID=test-app1660
6. Deployed containers in GCR to my-cluster. I editted the datanode to have two replicas while the rest were editted to have 1 replicas.
> For hadoop namenode and datanodes I had to add environment variables that were within this file: https://github.com/big-data-europe/docker-hadoop/blob/master/hadoop.env (First 9 Lines) I deployed these by going to container registry >> deploy >> added ports >> added env variables >> then continued<br/>
> Add this env variable for namenode: - CLUSTER_NAME=test
> Add this env variable for datanode: SERVICE_PRECONDITION: "namenode:9870"
> Example Commands for Jupyter (Variation of these commands were used for all containers except for hadoop datanodes and namenode): <br/>
> kubectl create deployment jupyter --image=jupyter/datascience-notebook 
> kubectl scale deployment jupyter --replicas=1
> kubectl autoscale deployment jupyter --cpu-percent=10 --min=1 --max=5
7. Edit yaml files so they use 250m cpu and 1Gi memory so that I had enough for all 
8. Expose the pods.
> Jupyter was exposed to port 8888 <br/>
> Sonarqube: 9000 <br/>
> Main application: 8080 <br/>
> Spark: 8080 <br/>
> Hadoop: 9870:9000 <br/>
> Example Command for Jupyter: kubectl expose deployment jupyter --name=jupyter-service --type=LoadBalancer --port 80 --target-port 8888 <br/>
    *For hadoop I manually created the service so I could expose both ports.
9. Update the Main Terminal Application so that it displays the links for all the pods 
> Commands: kubectl get svc (to see the external ip addresses) <br/>
> docker build -t us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 . <br/>
> docker push us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 <br/>
> kubectl set image deployment/hello-app hello-app=us-east1-docker.pkg.dev/${PROJECT_ID}/hello-repo/hello-app:v2 (This line was executed after I changed print statements to have the external IP addresses) <br/>

How to Use Application:
> Navigate to 34.75.201.101<br/>
> Follow Instructions on page<br/>

**A CLOSE VARIATION OF EXAMPLE COMMANDS WERE USED FOR ALL CONTAINERS/DEPLOYMENTS/PODS UNLESS I SPECIFIED IT WAS NOT USED FOR A SPECIFIC ONE**
