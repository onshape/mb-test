#!groovy

stage 'ONE'
node('master') {
  checkout scm
  echo env.BRANCH_NAME
  sh 'cat Jenkinsfile'
  sh 'stat file1'
  archive includes: 'file1'
}
