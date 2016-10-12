#!groovy

// Global variables
def LIB

currentBuild.rawBuild.project.description = description()

stage name: 'TEST'
//node('osx-bigmac-slave') {
node('master') {
    echo onshape.estimatedDuration as String
    echo groovy.json.JsonOutput.toJson(onshape.culprits)
    echo onshape.onshapeCulpritAddresses.join(',')
    echo onshape.otherCulpritAddresses.join(',')
    echo onshape.lsbCommit
    properties [[$class: 'DisableConcurrentBuildsJobProperty']]
    currentBuild.parent.concurrentBuild = false
    sh 'sleep 3'
    echo 'done'
}

// stage name: 'TEST2'
// //node('osx-bigmac-slave') {
// node('master') {
//     echo onshape.estimatedDuration as String
//     echo groovy.json.JsonOutput.toJson(onshape.culprits)
//     echo onshape.onshapeCulpritAddresses.join(',')
//     echo onshape.otherCulpritAddresses.join(',')
//     echo onshape.lsbCommit
//     properties [[$class: 'DisableConcurrentBuildsJobProperty']]
//     sh 'sleep 180'
//     echo 'done'
//     echo 'done'
//     echo 'done'
// }

// node('master') {
//   checkout scm

//   LIB = load 'buildSrc/jenkins/pipeline/lib.jenkinsfile'
//   step LIB.allowClaim()

//   def lsb = LIB.getBranchLsb(env.BRANCH_NAME)
//   // Global variable does not work inside string interpolation, use binding instead.
//   binding.setVariable('LSB_NUMBER', lsb.number.toString())

//   def lsbSha = LIB.getBuildSha(lsb)
//   // to avoid serialization error
//   lsb = null

//   currentBuild.rawBuild.description = "newton ${env.BRANCH_NAME} build ${LSB_NUMBER} ${lsbSha}"
//   def mustRun = true
//   for (b in currentBuild.rawBuild.project.builds) {
//     // this is wrong, we don't care about out sha, we care about the lsb sha of that build
//     // Jenkins.instance.getItem('promote-to-s3').getItem('pkania-pipeline').builds[0].getAction(hudson.model.ParametersAction).getParameter('BUILD_NUMBER').value
//     if ((LIB.getBuildSha(b) == lsbSha) && (b.result == hudson.model.Result.SUCCESS)) {
//       currentBuild.rawBuild.description += ' already passed'
//       mustRun = false
//     }
//   }
//   if (mustRun) {
//     LIB.mailOnError name: 'long-upgraded-retrieval-test', sh: bashCommand(), archive: [includes: '**/*.log,stage/fileretrieval/**/*'], junit: LIB.junitResults('stage/**/test-results/*.xml')
//   }
// }

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
