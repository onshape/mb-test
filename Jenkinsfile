#!groovy

stage 'ONE'
node('master') {
  checkout scm
  echo env.BRANCH
  sh 'cat Jenkinsfile'
}
