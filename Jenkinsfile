node {

    //Define all variables
    def appName = 'guess'
    def serviceName = "${appName}"  
    def imageTag = "meir215/${appName}"
    def buildnum = "1.0.${env.BUILD_NUMBER}"

    //Checkout Code from Git
    checkout scm
    
    //master : Build the docker image.
    stage('Build image') {
        env.BRANCH_NAME == 'master'
        sh("docker build -t ${imageTag}:latest .")
        }
    
    //master : E2E testing
    stage('E2E testing') {
        env.BRANCH_NAME == 'master'
         sh 'echo test'
        }

    //master : Push the image to docker hub container registry
    stage('Push image to registry') {
        env.BRANCH_NAME == 'master'
        withDockerRegistry([ credentialsId: "docker", url: "" ]) {
        sh("docker push ${imageTag}:latest")
        }
        }

    //feature build and test
    stage('feature build and test') {
        env.BRANCH_NAME == 'feature'
        sh("docker build -t ${imageTag}:${buildnum} .")
        sh("docker run -d ${imageTag}:${buildnum}")
        sh 'echo feature test'
        }

    //dev build and test
        stage('feature or dev build and test') {
        env.BRANCH_NAME == 'dev'
        sh("docker build -t ${imageTag}/snapshot:${buildnum} .")
        sh("docker run -d ${imageTag}/snapshot:${buildnum}")
        sh 'echo dev test'
        }
}