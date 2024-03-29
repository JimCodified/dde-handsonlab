# Link Extractor: Step 6 - Distributing the Docker App

In Step 4 and 5 we have been showing how a Docker Application works on both
Docker Swarm and on Kubernetes locally on Docker Desktop Enterprise. This is a
great experience for developers to work on their applications without requiring
any external infrastructure. The final part of this lab is where we are going to
"Share" or distribute our application.

Docker Applications can actually be distributed the same way Container images
can be distributed through the simple `push` and `pull` syntax.

> In this section of the lab, you will need a Docker ID. If you do not have a
> Docker ID head to https://hub.docker.com/signup to set one up.

## Try it out

1) First, we need to login to Docker Desktop using your Docker ID.

   ```powershell
   docker login <hub-id>
   ```

2) Docker Applications can be pushed and pulled as OCI container images. This
   means a Docker Application can be distributed through any container registry.
   We are going to leverage the Docker Hub (a SaaS container registry) to
   distribute our Docker Application.

   A simple `docker app push` will embed the local file structure into an OCI
   image and push this to a container registry.

   ```powershell
   > docker app push linkextractor.dockerapp --tag <hub-id>/linkextractor:v1
   ```

   Feel free to browse to the Docker Hub and checkout our pushed Docker App.

   https://hub.docker.com/r/<hub-id>/linkextractor
   https://hub.docker.com/r/<hub-id>/ee-templates-web
   https://hub.docker.com/r/<hub-id>/ee-templates-api

3) Now you can delete all local copies of the Link Extractor app as it is now
   stored in the Docker Hub.

   ```powershell
   rm linkextractor.dockerapp
   docker image rm <hub-id>/linkextractor:v1-invoc
   ```

4) Now can inspect our remote Docker App. 

   ```powershell
   > docker app inspect <hub-id>/linkextractor:v1
   linkextractor 0.1.0
   
   Maintained by: Gordon the Turtle
   
   sample description
   
   Services (2) Replicas Ports Image
   ------------ -------- ----- -----
   api          1        5000  ollypom/linkextractor@sha256:5c85bd896335b6ebc08c95d573ab80d3ef0275cfd61688ca39e7dfbd5be2a4ba
   www          1        80    ollypom/linkextractor@sha256:86aba1f516b447634ba56b8c4dd347ae38e87221e8225a4765245817ce271a89
   
   Parameters (4) Value
   -------------- -----
   api.port       5000
   api.replicas   1
   www.port       80
   www.replicas   1
   
   Attachment (1)            Size
   --------------            ----
   parameters/production.yml 63B
   ```

5) Finally, we can pull down our remote Docker App and run it locally without
   the files extracted on to our system. We do need to enable a Container
   Orchestrator though to run out application.

   ```
   > docker swarm init
   
   > docker app install <hub-id>/linkextractor:v1
   WARNING: installing over previously failed installation "linkextractor"
   Creating network linkextractor_default
   Creating service linkextractor_api
   Creating service linkextractor_www
   Application "linkextractor" installed on context "default"
   ```

   For a final time, open a web browser and navigate to http://localhost

6) Congratulations you have finished the lab!!!