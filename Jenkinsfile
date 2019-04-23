
node {
  checkout scm
  $pomFile = "k8s.yaml"
  pomPath = findFiles(glob: "**/k8s.yaml")[0].path
  env.WORKSPACE = pwd()
  def projectName = new File(pomPath).parent
  baseDir = "${env.WORKSPACE}/$projectName"
}