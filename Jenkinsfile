node {
  checkout scm
  label 'ontrack' 

  // Take the latest id to will use as image tag
  sh "git rev-parse --short HEAD > commit-id"
  tag = readFile('commit-id').replace('\n', '').replace('\r', '')

  // Set tge application name, the repository address and the image name with version
  appName = 'node-test'
  registryHost = 'registry-pusher.medikar.com.br/'
  imageName = "${registryHost}${appName}:${tag}"
  k8sfile = "https://raw.githubusercontent.com/lucca-cardial/node-deploy-jenkins/master/k8s.yaml"
  sh mkdir /root/test-access 
  sh cp $WORKSPACE/build/ontrace.build /root/test-access

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
