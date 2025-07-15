node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update manifest & Git push') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIAL', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {

                    def manifestPath = "./administrationmanifestfiles/front/deployment-front.yaml"

                    sh '''
                        git config user.email "brahimcnss2025@gmail.com"
                        git config user.name "brahimcnss"
                    '''

                    // Modifier le tag de l’image
                    sh "sed -i 's|image: .*|image: 192.168.2.19:5000/frontadmine:${DOCKERTAG}|' ${manifestPath}"

                    // Commit + push
                    sh """
                        git add ${manifestPath}
                        git commit -m 'CD: update frontadmine to ${DOCKERTAG} [skip ci]'
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/brahimcnss/cnssbrazza.git HEAD:preprod
                    """
                }
            }
        }
    }
}
