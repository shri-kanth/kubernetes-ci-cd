node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    commitTag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}"
    env.BUILDIMG=imageName
    
    env.COMMIT_TAG=commitTag    
    sh 'printf "\nENV BUILD_ID "$COMMIT_TAG >> applications/hello-kenzan/Dockerfile'
    sh "sed -i 's/{GIT_COMMIT}/${commitTag}/g' applications/${appName}/k8s/*.yaml"
    sh 'cat applications/hello-kenzan/Dockerfile'
    sh 'cat applications/hello-kenzan/k8s/deployment.yaml'
    
    stage "Build"
    
        sh "docker build -t ${imageName}:${commitTag} -t ${imageName}:latest -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}:latest"

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
