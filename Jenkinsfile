node {
  //
  def scmVars
  def pullRequest = false
  def commitSha
  def buildBranch
  def pullId
  def targetBranch
  def changes = ''
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
      targetBranch = params.ghprbTargetBranch
    } else {
      // PUSH build
    }
    //
    println('SCM variables')
    println(scmVars)
    println('Job input parameters')
    println(params)
    println('Build info')
    println("[PR:${pullRequest}] [BRANCH:${buildBranch}] [COMMIT: ${commitSha}] [PULL ID: ${pullId}] [TARGET BRANCH: ${targetBranch}]")
    println('Environment variables')
    println(sh(script:'env', returnStdout: true))    
    //
    if (pullRequest) {
      def script = "git diff --name-only HEAD ${targetBranch} | awk -v FS='/' '{print $2}' | sort -u | tr '\n' ' '"
      changes = sh(script:"git diff --name-only HEAD ${targetBranch} | awk -v FS='/' '{print $2}' | sort -u | tr '\n' ' '", returnStdout: true)
      println('Changes')
      println(changes)
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
