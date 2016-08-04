#!groovy

// Global variables
binding.setVariable('ALLOW_CLAIM', [$class: 'ClaimPublisher'])
currentBuild.rawBuild.project.description = description()

stage name: 'TEST', concurrency: 1
//node('osx-bigmac-slave') {
node('master') {
  checkout scm

  def lib = load 'buildSrc/jenkins/pipeline/lib.jenkinsfile'
  step lib.allowClaim()

//   def lsb = Jenkins.instance.getItem('newton').getItem(env.BRANCH_NAME).lastSuccessfulBuild
  def lsb = Jenkins.instance.getItem('newton-osx').getItem(env.BRANCH_NAME).lastSuccessfulBuild
  def lsbNumber = lsb.number.toString()
  binding.setVariable('LSB_NUMBER', lsbNumber)

  def lsbSha = lib.getBuildSha(lsb)
  lsb = null

  currentBuild.rawBuild.description = "newton ${env.BRANCH_NAME} build ${lsbNumber} ${lsbSha}"
  def mustRun = true
  for (b in currentBuild.rawBuild.project.builds) {
    if ((lib.getBuildSha(b) == lsbSha) && (b.result == hudson.model.Result.SUCCESS)) {
      currentBuild.rawBuild.description += ' already passed'
      mustRun = false
    }
  }
  if (mustRun) {
    lib.mailOnError sh: bashCommand(), archive: [includes: '**/*.log,stage/fileretrieval/**/*'], junit: lib.junitResults('stage/**/test-results/*.xml')
  }
}

def description() {
  """
Find version <em>of last succsessful ${env.BRANCH_NAME} build</em> and then trigger build of
<ul>
  <li><b>${env.BRANCH_NAME}-long-upgraded-retrieval-test-darwin-x64-RelWithDebInfo</b></li>
</ul>
at that version
"""
}

def bashCommand() {
  """# We need tell gradle which BUILD_NUMBER and GIT_BRANCH to use.
export BUILD_NUMBER=${LSB_NUMBER}
export GIT_BRANCH=${env.BRANCH_NAME}

export RELEASE=1
./buildSrc/jenkins/distributed/jenkins_long_upgraded_retrieval_test.bash
python tools/retrieval2signal.py

"""
}
// keep builds 10 builds
