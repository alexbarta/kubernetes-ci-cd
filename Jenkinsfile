node {
    def app
  
    checkout scm
  
    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "docker.io/barta/"
    registryCredential = "dockerhub"
    //imageName = "${registryHost}${appName}:${tag}"
    imageName = "${registryHost}${appName}"
    env.BUILDIMG=imageName

    stage "Build"
    
        //sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
        app = docker.build(${imageName}, "applications/hello-kenzan/Dockerfile")

    stage "Push"

        //sh "docker push ${imageName}"
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
        app.push("${env.BUILD_NUMBER}")
        app.push("latest")

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
