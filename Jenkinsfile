
node {
  checkout scm
  pomPath = findFiles(glob: "**/k8s.yml")[0].path
  env.WORKSPACE = pwd()
  def projectName = new File(pomPath).parent
  baseDir = "${env.WORKSPACE}/$projectName"
  echo baseDir
}