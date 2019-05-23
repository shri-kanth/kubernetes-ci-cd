node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    commitTag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}"
    env.BUILDIMG=imageName
    sh 'echo ENV BUILD_ID ${commitTag} >> application/hello-kenzan/Dockerfile'
    sh 'cat application/hello-kenzan/Dockerfile'
    stage "Build"
    
        sh "docker build -t ${imageName}:${commitTag} -t ${imageName}:latest -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}:latest"

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
