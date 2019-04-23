
node {
  checkout scm
  label 'ontrack' 
  def workspace = pwd()
  // Take the latest id to will use as image tag
  sh "git rev-parse --short HEAD > commit-id"
  tag = readFile('commit-id').replace('\n', '').replace('\r', '')

  // Set tge application name, the repository address and the image name with version
  appName = 'node-test'
  registryHost = 'registry-pusher.medikar.com.br/'
  imageName = "${registryHost}${appName}:${tag}"
  k8sfile = "${workspace}/k8s.yaml"

  // Define Pepiline

  stage "Build"
    def  customImage = docker.build("${imageName}")

  stage "Push"
    customImage.push()
  
  stage "Deploy PROD"
  input "Deploy to PROD?"
  customImage.push('latest')
  sh "/snap/bin/kubectl apply -f ${k8sfile}"
  sh "/snap/bin/kubectl set image deployment react-app react-app=${imageName} --record"
  sh "/snap/bin/kubectl rollout status deployment/react-app"
}