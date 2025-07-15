node {
    def app
  
    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIAL', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                         //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email brahimcnss2025@gmail.com"
                        sh "git config user.name brahimcnss"
                        //sh "git switch master "
                        sh "cat ./administrationmanifestfiles/front/deployment-front.yaml" //affricher le contenu
                        sh "sed -i 's+192.168.2.19:5000/frontadmine.*+192.168.2.19:5000/frontadmine:${DOCKERTAG}+g' ./administrationmanifestfiles/front/deployment-front.yaml"                       
                        sh "cat ./administrationmanifestfiles/front/deployment-front.yaml"
                        sh "git add ." 
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'" 
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/cnssbrazza.git HEAD:preprod"
                       
                        
                        
      }
    }
  }
}
}
