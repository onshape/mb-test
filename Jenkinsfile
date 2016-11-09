#!groovy

binding.setVariable('ONSHAPE_MAIL_TO', 'pkania@onshape.com')
binding.setVariable('ONSHAPE_SLACK_CHANNEL', '@pkania')

stage 'ONE'
node('master') {
  checkout scm
  echo env.BRANCH_NAME
  binding.setVariable('EXTRA_ENV', [])

  wrapBuild sh:'false'
}

def wrapBuild(params) {
  def savedErrors = []
  try {
    if (params.scm) {
      // call the scm closure
      (params.scm)()
    } else {
      git branch: onshape.branchName, url: "git@github.com:onshape/${onshape.folderName}.git"
    }
    withEnv(EXTRA_ENV) {
      if (params.timeout) {
        timeout(params.timeout) {
          sh params.sh
        }
      } else {
        sh params.sh
      }
    }
  }
  catch (err) {
    savedErrors << err
  }
  finally {
    if (params.archive) {
      // archive step does not throw an exception
      archive params.archive
    }
    // junitResults does throw an exception
    try {
      if (params.junit) {
        step(params.junit)
      }
    }
    catch (err) {
      savedErrors << err
    }
    finally {
      try {
        if (params.jacoco) {
          step(params.jacoco)
        }
      }
      catch (err) {
        savedErrors << err
      }
      finally {
        if (savedErrors) {
          onshape.handleErrors(params.name, savedErrors, true)
        }
      }
    }
  }
}
