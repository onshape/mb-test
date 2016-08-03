#!groovy

stage 'ONE'
node('master') {
  checkout scm
  for (p in env) {
    echo "${p.key}=${p.value}"
  }
  echo env.BRANCH_NAME
  echo env.GIT_COMMIT
  echo pwd()
  sh 'stat file1'
  archive includes: 'file1'
  def lsb = currentBuild.rawbuild.project.lastSuccessfulBuild
  def lsbNumber = lsb.number.toString()
  def gitData = lsb.actions.find { it instanceof hudson.plugins.git.util.BuildData }
  def lsbSha = gitData.lastBuiltRevision.sha1String
  echo lsbSha
}

