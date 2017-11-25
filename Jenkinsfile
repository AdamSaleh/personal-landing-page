node {
  stage ('Build') { 
    step([$class: 'WsCleanup'])
    checkout scm
    sh 'nikola build' 
    dir('output') {
      withAWS(credentials:'jenkinss3', region:'eu-central-1') {
        if (env.BRANCH_NAME && env.BRANCH_NAME!='master') {
          s3Upload path: env.BRANCH_NAME, acl: 'PublicRead', bucket: 'asaleh.net', includePathPattern: '**'
        } else if (env.BRANCH_NAME && env.BRANCH_NAME=='master') {
          s3Upload path: '', acl: 'PublicRead', bucket: 'www.asaleh.net', includePathPattern: '**'
        }
      }
    }
  }
}