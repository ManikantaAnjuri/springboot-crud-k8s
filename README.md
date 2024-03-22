# springboot-crud-k8s
Create a JAR file out of source code.
To Create JAR file, we need to skip the tests associated with the code using the below command. Because it is xtrying to connect to database while building the JAR File, in our case it is i.e.,database is running on Conatiner.

1. mvn clean package -DskipTests -U
    or
   we can update the same in docker instead of building JAR file separately.

2. Docker build . -t <nameof docker image> .
3. docker push <nameofrepo>

4. Fisrt Database Container should be up and running before running our application po.
    kubectl apply -f Mysql-secrets.yml ,
    kubectl apply -f mysql-configMap.yml
    Kubectl apply -f db-deployment.yml

6. Create the application deployment file.
    Kubectl apply -f app-deployment.yml

7. Exec into the mysql container and login with the given creds in my-secrets.yml file(Secrets values should be encoded first and added in the secret.yml)
    kubectl exec -it nameofpod /bin/bash
   - mysql -h mysql -u uname(replace with uname) -p password (replace with passwd)
   IF you are able to login then it is working fine. We can procced further.

8. Now exec into app container
   - Kubectl exec -it <nameofappcontainer> /bin/bash
   - curl localhost:<nodeport> (node port svc)
   to verify if our app is running inside our or not

9. If you are running all the above setup in Minikube then we need to an additional step to access the appliaction on browser.
   Need to use port forwarding here, reason behind this is as MINIKUBE runs as a docker container inside the HOST.
   So, with the docker container IP , we cannot access our app out side that docker container.
   If we enter minikube IP, it gives the IP of docker container where Minikube is running.

10. kubectl port-forward svc/springboot-crud-svc 3000(we will access on this port using node IP):8080(SVC PORT of appliaction) --address 0.0.0.0 &

11. If you are running application directly on open source k8s we can access application on publicip:Nodeport Number



