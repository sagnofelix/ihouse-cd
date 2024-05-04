node {
    def app

    stage('Clone the ihouse cd repository') {
        checkout scm
    }

    stage('Update GIT') {
      script {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
            //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
            sh "git config user.email sagno.felix.sf@gmail.com"
            sh "git config user.name sagnofelix"
            //sh "git switch master"
            sh "cat deployment.yaml"
            sh "sed -i 's+jeanfelixsagno/ihouse.*+jeanfelixsagno/ihouse:${DOCKERTAG}+g' deployment.yaml"
            sh "cat deployment.yaml"
            sh "git add ."
            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER} for version: ${DOCKERTAG}'"
            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/argocd.git HEAD:main"
          }
        }
      }
    }
}