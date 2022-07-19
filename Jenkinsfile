node {
  //
  def scmVars
  def pullRequest = false
  def commitSha
  def buildBranch
  def pullId
  //
  stage('Checkout') {
    //
    scmVars = checkout scm
    //
    // Create config for detached build
    sh "echo '{\"detach\": true}' > '.tlnrc'"
    //
  }

  try {
    //
    commitSha = scmVars.GIT_COMMIT
    buildBranch = scmVars.GIT_BRANCH
    if (buildBranch.contains('PR-')) {
      // multibranch PR build
      pullRequest = true
      pullId = env.CHANGE_ID
    } else if (params.containsKey('sha1')){
      // standard PR build
      pullRequest = true
      pullId = params.ghprbPullId
      commitSha = params.ghprbActualCommit
    } else {
      // PUSH build
    }
    
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
