
node {
  checkout scm
  pomPath = findFiles(glob: "**/k8s.yml").path
  echo pomPath
  env.WORKSPACE = pwd()
  def projectName = new File(pomPath).parent
  baseDir = "${env.WORKSPACE}/$projectName"
  echo baseDir
}