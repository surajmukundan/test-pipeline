pipeline {
  agent {
    kubernetes {
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'jenkins-slave'  // define a default container if more than a few stages use it, will default to jnlp container
      label 'jenkins-slave'
    }
  }
  stages {
    stage('Build') {
      steps {  // no container directive is needed as the maven container is the default
        sh "mvn clean package"   
      }
    }
    stage('Build Docker Image') {
      steps {
        container('docker') {  
          sh "docker build -t surajmukundan/promo-app:dev ."  // when we run docker in this step, we're running it via a shell on the docker build-pod container, 
          sh "docker push surajmukundan/promo-app:dev"        // which is just connecting to the host docker deaemon
        }
      }
    }
  }
}
