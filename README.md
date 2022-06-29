# Git-Lab-CI-CD

https://raw.githubusercontent.com/RodrigoMvs123/Git-Lab-CI-CD/main/README.md

https://github.com/RodrigoMvs123/Git-Lab-CI-CD/blame/main/README.md


Git Lab CI CD 
https://www.twingate.com/


https://youtu.be/qP8kir2GUgo

Pyhton App ( Ubunto Server ) 

Run Tests 
Build Docker Image
Push to Docker Registry 
Deploy to Server 

GitLab

Manage 
Plan 
Create
Verify 
Package 
Secure 
Release 
Configure 
Monitor 
Protect 

Continuous Integration 
Continuous Deployment ( Delivery ) 
Code Changes / Teste / Build / Release 

Merge code changes 
Remote git repository 
Tests
Build and package 
Artifact repository 
Deployment  

Azure Pipelines
Pipeline
TeamCity: o servidor de CI e CD sem dor de cabeça da JetBrains
Travis CI
AWS CodePipeline | Integração e entrega contínuas

GitLab Repository 
Repository | GitLab
Manage projects | GitLab

 
     Pyhton App ( Ubunto Server ) 

git clone e76fdb4b28d4ebb0c3036eda986aa7d03e0970e6 ( https://gitlab.com/nanuchi/gitlab-cicd-crash-course ) 
cd gitlab-cicd-crash-course 
ls
code. 
Visual Studio Code
src > app > > tests 

terminal 
make test
export PORT=5004 
make run 
running on http://127.0.0.1:5004 ( Copy and past on Browser ) 

PipeLine Configuration File
PipeLine is Written in Code
Hosted inside application´s git repository 
Whole CI / CD configuration is written in YAML format ( .gitlab-ci-yml ) / New File 

Run Tests / Build Image / Deploy / … ( Jobs ) 

make command available
pip available 
python available 

run_tests: 
   image: python:3.9-slim-buster ( https://hub.docker.com/_/python ) 
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

Shell Executer 
Docker Machine ( Install a container ) / Docker Image Node.js & npm available inside the container / alpine Linux Commands available 

Gitlab manage runners use a Rubi Image to start the container //

 Gitlab Repository 
run_tests: 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 
CI /CD ( Running ) 
Build and Push Docker Image 
Repository ( GitLab ) / Repository | GitLab
CI/CD ( Editor ) 
Docker Hub ( Private Image Registry ) // Copy 

Variables for login credentials 
Docker Login 
username passport ( Private Repository ) 

Settings / Administrator / User 
CICD / Add Variable / Registry User / Passport 
Key
Username / Docker Hub  
Masked Variable

Write Pipeline Configuration 
Editor ( CI/CD ) 


Gitlab Repository 

 variables:
     IMAGE_NAME:nanajanashia/demoapp
     IMAGE_TAG: python-app-1.0 

run_tests: 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

build_image: 
      
    before_script: 
docker login -u $REGISTRY_USER - p REGISTRY_PASS


        script: 
docker build . -t $DOCKER_IMAGE:$DOCKER_TAG :   //tag image using repository name / complete name of the image
docker $DOCKER_IMAGE:$DOCKER_TAG . 




Docker in Docker 


Gitlab Repository 

 variables:
     IMAGE_NAME:nanajanashia/demoapp
     IMAGE_TAG: python-app-1.0 

run_tests: 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

build_image: 
      image: docker: 20.10.16
        services: 
docker: 20.10.16-dind ( Docker Cliente / Docker Daemon ) 
    variables: 
   DOCKER_TLS_CERTDIR: “/certs”   
 before_script: 
docker login -u $REGISTRY_USER - p REGISTRY_PASS


        script: 
docker build . -t $DOCKER_IMAGE:$DOCKER_TAG :   //tag image using repository name / complete name of the image
docker $DOCKER_IMAGE:$DOCKER_TAG . 
Execute Pipeline ( GitLab ) 
Build Image ( and Push ) 
Run Tests 

Deploy 

Stages 
Editor 

Gitlab Repository 

 variables:
     IMAGE_NAME:nanajanashia/demoapp
     IMAGE_TAG: python-app-1.0 

stages:
test
build



run_tests: 
stage: test 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

build_image: 
stage: build

      image: docker: 20.10.16
        services: 
docker: 20.10.16-dind ( Docker Cliente / Docker Daemon ) 
    variables: 
   DOCKER_TLS_CERTDIR: “/certs”   
 before_script: 
docker login -u $REGISTRY_USER - p REGISTRY_PASS


        script: 
docker build . -t $DOCKER_IMAGE:$DOCKER_TAG :   //tag image using repository name / complete name of the image
docker $DOCKER_IMAGE:$DOCKER_TAG . 
Pipeline ( status ) // test / build ( Stages ) 



Prepare Deployment Server 
Create Ubuntu Server on Digital Ocean : How To Set Up an Ubuntu 20.04 Server on a DigitalOcean Droplet ( AWS ) 
public key / deploymen-server encrypted communication ssn username@host / private key 

cd.. 
ssh-keygen
ls /Users/nanajanashia/.ssh | grep digital 

digital_ocean_key 
digital_ocean_key.pub 

cat digital_ocean_key.pub 
Users/nanajanashia/.ssh digital_ocean_key.pub 
… ( Public Key ) 
Digital Ocean / Settings / Security 
Add SSH Key - … ( Public Key )  ( Deployment-server-key

Droplets 
Ubuntu 
Regular with SSD ( Basic ) 
Region ( Frankfurt ) 
Public IP Address ( Copy ) 
ssh -i ~/.ssh/digital_ocean_key root@161.35.223.117 ( private Key SSH  ) 
yes

docker 
apt update 
apt install docker.io
Y

Docker ps
exist 

Deploy APP
GitLab Pipeline 
Settings CICD 
Variables ( Add ) SSH_KEY 
cat ~/.ssh/digital_ocean_key 
// … private Key SSH ( Copy ) 
// … private Key SSH ( Copy ) ( Value ) 
type ( Variable / File ) - File 

editor ( GitLab ) 



Gitlab Repository 

 variables:
     IMAGE_NAME:nanajanashia/demoapp
     IMAGE_TAG: python-app-1.0 

stages:
test
build
deploy



run_tests: 
stage: test 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

build_image: 
stage: build

      image: docker: 20.10.16
        services: 
docker: 20.10.16-dind ( Docker Cliente / Docker Daemon ) 
    variables: 
   DOCKER_TLS_CERTDIR: “/certs”   
 before_script: 
docker login -u $REGISTRY_USER - p REGISTRY_PASS


        script: 
docker build . -t $IMAGE_NAME:$IMAGE_TAG :   //tag image using repository name / complete name of the image
docker push $IMAGE_NAME:$IMAGE_TAG . 

deploy: 
stage: deploy 
script: 
ssh - o StrictHostKeyCheking=no -i $SSH_KEY root@161.35.223.117”
             docker login -u $REGISTRY_USER - p REGISTRY_PASS &&
             docker ps -aq | xargs docker stop | xargs docker rm && 
             docker run - p 5000:5000 $IMAGE_NAME:$IMAGE_TAG”
ls -l ~/.ssh/digital_ocean_key  




Gitlab Repository 

 variables:
     IMAGE_NAME:nanajanashia/demoapp
     IMAGE_TAG: python-app-1.0 

stages:
test
build
deploy



run_tests: 
stage: test 
   image: python:3.9-slim-buster
   before_script:
apt-get update && apt-get install make 
   script: 
make test 

build_image: 
stage: build

      image: docker: 20.10.16
        services: 
docker: 20.10.16-dind ( Docker Cliente / Docker Daemon ) 
    variables: 
   DOCKER_TLS_CERTDIR: “/certs”   
 before_script: 
docker login -u $REGISTRY_USER - p REGISTRY_PASS


        script: 
docker build . -t $IMAGE_NAME:$IMAGE_TAG :   //tag image using repository name / complete name of the image
docker push $IMAGE_NAME:$IMAGE_TAG . 

deploy: 
stage: deploy 
before_script
chmod 400 $SSH_KEY
script: 
ssh - o StrictHostKeyCheking=no -i $SSH_KEY root@161.35.223.117”
             docker login -u $REGISTRY_USER - p REGISTRY_PASS &&
             docker ps -aq | xargs docker stop | xargs docker rm && 
             docker run -d - p 5000:5000 $IMAGE_NAME:$IMAGE_TAG”
ls -l ~/.ssh/digital_ocean_key  


Validate Application Runs 

ssh - o StrictHostKeyCheking=no -i $SSH_KEY root@161.35.223.117”
docker ps 
161.35.223.117:5000 ( Browser ) 

Delete your server on DO ( Delete droplets ) 

Artifacts ( Testing reports, passing data ) 
Caching ( Speed up pipeline and save costs ) 
Job Templates ( Avoid code duplication ) 
Set up own GitLab Runner ( Local and remote ) 
Use build-in Docker Registry 
Docker Compose 
Kubernetes ( Deploy to kubernetes cluster )
Microservices ( For both monorepo and polyrepo ) 
Multi-Stage 
Dynamic Versioning 
Configure SAST Tests 


  

