
node {
  checkout scm
  def pomPath = findFiles(glob: "**/k8s.yaml")[0].path
  echo new File(env.WORKSPACE, pomPath).getParent() +"\k8s.yaml"
}