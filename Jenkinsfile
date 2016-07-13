#!groovy

stage 'ONE'
node('master') {
  checkout scm
  for (p in env) {
    echo "${p.key}=${p.value}"
  }
  echo pwd()
  sh 'cat Jenkinsfile'
  sh 'stat file1'
  archive includes: 'file1'
}

build job: 'promote-to-s3/api-created', parameters: [[$class: 'StringParameterValue', name: 'BRANCH_NAME', value: env.BRANCH_NAME]], quietPeriod: 0
