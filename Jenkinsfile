pipeline {
 tools {
 maven 'Maven3'
 }
 agent any
 environment {
 registry = "051826706788.dkr.ecr.ap-south-1.amazonaws.com/mydockerrepo"
 }
 
 stages {
 stage('Cloning Git') {
 steps {
 git 'https://github.com/namagiriboobalan/docker-spring-boot.git' 
 }
 }
 stage ('Build') {
 steps {
 sh "mvn clean install -DskipTests=true" 
 }
 }
 // Building Docker images
 stage('Building image') {
 steps{
 script {
 dockerImage = docker.build registry 
 dockerImage.tag("$BUILD_NUMBER")
 }
 }
 }
 stage('Pushing to ECR') {
 steps{ 
 script {
 sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 051826706788.dkr.ecr.ap-south-1.amazonaws.com'

  sh 'docker push 051826706788.dkr.ecr.ap-south-1.amazonaws.com/mydockerrepo:$BUILD_NUMBER'
 }
 }
 }
 stage ('Helm Deploy') {
 steps {
 script {
 sh "helm upgrade first --install mychart --namespace helm-deployment --set image.tag=$BUILD_NUMBER"
 }
 }
 }
 }
}   
