node {
  //
  def scmVars
  def pullRequest = false
  def commitSha
  def sourcedBranch
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
    sourcedBranch = scmVars.GIT_BRANCH
    if (sourcedBranch.contains('PR-')) {
      // multibranch PR build
      pullRequest = true
      pullId = env.CHANGE_ID
    } else if (params.containsKey('sha1')){
      // standard PR build
      pullRequest = true
      sourcedBranch = params.GIT_BRANCH
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
    println("[PR:${pullRequest}] [SOURCE BRANCH:${sourcedBranch}] [COMMIT: ${commitSha}] [PULL ID: ${pullId}] [TARGET BRANCH: ${targetBranch}]")
    println('Environment variables')
    println(sh(script:'env', returnStdout: true))    
    //
    if (pullRequest) {
      changes = sh(script:"git diff --name-only HEAD origin/${targetBranch}", returnStdout: true)
    }
    println('Changes')
    println(changes)
    
    
    
    stage('Setup build environment') {
      sh '''
tln prereq-app
      '''
    }

    stage('Build web/portal') {
      if (changes.contains('web/portal') || !pullRequest) {
        sh '''
tln init:build:test web/portal
        '''
      }
    }
    stage('Build services/auth') {
      if (changes.contains('ervices/auth') || !pullRequest) {
        sh '''
tln build:test services/auth
        '''
      }
    }

    stage('SonarQube analysis') {
    }

    stage('Delivery') {
      if (pullRequest){
      } else {
        // create docker, push artifacts to the Harbor/Nexus/etc.
        // archiveArtifacts artifacts: 'path/2/artifact'
      }
    }

    stage('Deploy') {
      if (pullRequest){
      } else {
      }
    }
  } catch (e) {
    def traceStack = e.toString()
    throw e
  }
}
