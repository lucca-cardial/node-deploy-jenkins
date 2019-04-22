node {
  agent { label 'ontrack' }
  checkout scm

  // Take the latest id to will use as image tag
  sh "git rev-parse --short HEAD > commit-id"
  tag = readFile('commit-id').replace('\n', '').replace('\r', '')

  // Set tge application name, the repository address and the image name with version
  appName = 'node-test'
  registryHost = 'registry-pusher.medikar.com.br/'
  imageName = "${registryHost}${appName}:${tag}"
  k8sfile = "https://raw.githubusercontent.com/lucca-cardial/node-deploy-jenkins/master/k8s.yaml"

  // Define Pepiline

  stage "Build"
    def  customImage = docker.build("${imageName}")

  stage "Push"
    customImage.push()
  
  stage "Deploy PROD"
  input "Deploy to PROD?"
  customImage.push('latest')
  sh "kubectl apply -f ${k8sfile}"
  sh "kubectl set image deployment app app=${imageName} --record"
  sh "kubectl rollout status deployment/app"
}
