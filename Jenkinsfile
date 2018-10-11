pipeline {
    agent {
        label "jenkins-nodejs"
    }
    environment {
      ORG               = 'dcole-inviqa'
      APP_NAME          = 'hello-world'
      CHARTMUSEUM_CREDS = credentials('jenkins-x-chartmuseum')
    }
    stages {
      stage('CI Build and push snapshot') {
        when {
          branch 'PR-*'
        }
        environment {
          PREVIEW_VERSION = "0.0.0-SNAPSHOT-PR-3-5"
          PREVIEW_NAMESPACE = "$APP_NAME-$BRANCH_NAME".toLowerCase()
          HELM_RELEASE = "$PREVIEW_NAMESPACE".toLowerCase()
        }
        steps {
          container('nodejs') {
            sh "export VERSION=$PREVIEW_VERSION"
          }
          dir ('./charts/preview') {
            container('nodejs') {
              sh "make preview"
              sh "jx step helm build --log-level=debug --verbose"
              sh "jx preview --app $APP_NAME --dir ../.."
            }
          }
        }
      }
    }
}
 