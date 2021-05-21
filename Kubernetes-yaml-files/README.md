1. Install shipyard using the link _https://shipyard.demo.gs/_ (Requires wget).
2. After installation run the command `export KUBECONFIG="$HOME/.shipyard/yards/shipyard/kubeconfig.yml"`.
3. Execute the command `yard up`.
   - This will initiate the kubernetes services.
   - Now we can perform various `kubectl` related commands.
4. For building Postgres image use the command `docker run -e POSTGRES_USER=akash -e POSTGRES_PASSWORD=abcd1234 -d postgres`.
   - The above command will run the image in demonised mode.
   - Execute `docker ps`, we will the Postgres container running.
   - Stop the container using the command `docker stop {container ID}`.
   - Get the container ID of the above stopped container using the command `docker ps -a`. 
   - Now build this image and push it to the docker hub using following command,
        `docker login` (Enter the credentials)
        `docker commit {container ID} {user_name}/{image_name}`
        `docker push {user_name}/{image_name}`
   - Use the modified image for the database deployment.
   - Similarly build an image of rails application and push it to docker hub and use that in application deployment. 
5. Create a deployment YAML file _deployment-meal.yaml_ for rails application and deploy it using command `kubectl apply -f deployment-meal.yaml`.
6. create deployment YAML file deployment-db.yaml for database and deploy it using the command `kubectl apply -f deployment-db.yaml`.
7. Check that the deployments are up and running using the command `kubectl get deployments` and to check the running pods use the command `kubectl get pods`.
8. Now create a services for application and database.
9. For application create a application service file _meal-svc-load.yaml_ and deploy it using the command `kubectl apply -f meal-svc-load.yaml`.
10. For database create a database service file _db-svc-cluster.yaml_ and deploy it using the command `kubectl apply -f db-svc-cluster.yaml`.
11. Check the services are up and running using the command `kubectl get svc`
12. For the application to reach the database we should provide the dns of database service in the database.yml file of application i.e "host: db-svc.default.svc.cluster.local"
13. Now execute the below commands for databse migration,
       `kubectl exec {pod_name} -- rake db:setup`
       `kubectl exec {pod_name} -- rake db:migrate` 
14. To access the application from outside set a port forward for the application service using the command `kubectl port-forward service/load-svc 3000:3000`.
15. Now the application will be listening from the localhost via port 3000.
