node {
  //
  stage('Checkout') {
    //
    def scmVars = checkout scm
    //
    // Create config for detached build
    sh "echo '{\"detach\": true}' > '.tlnrc'"
  }

  try {

    stage('Setup build environment') {
      sh '''
tln install web/portal --depends
tln install services/auth --depends
      '''
    }

    stage('Build') {
    }

    stage('Unit tests') {
    }

    stage('SonarQube analysis') {
    }

    stage('Delivery') {
      /*
      if (pull request){
      } else {
        // create docker, push artifacts to the Harbor/Nexus/etc.
        // archiveArtifacts artifacts: 'path/2/artifact'
      }
      */
    }

    stage('Deploy') {
      /*
      if (helper.pullRequest){
      } else {
      }
      */
    }
  } catch (e) {
    def traceStack = e.toString()
    throw e
  }
}
