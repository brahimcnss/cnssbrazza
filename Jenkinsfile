node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(
                    credentialsId: 'GITHUB_CREDENTIAL',
                    usernameVariable: 'GIT_USERNAME',
                    passwordVariable: 'GIT_PASSWORD'
                )]) {

                    sh '''
                        echo "🔧 Configuration de Git..."
                        git config user.email brahimcnss2025@gmail.com
                        git config user.name brahimcnss

                        echo "📄 Contenu avant modification :"
                        cat ./administrationmanifestfiles/front/deployment-front.yaml

                        echo "✏️ Mise à jour du tag Docker..."
                        sed -i 's+192.168.2.19:5000/frontadmine.*+192.168.2.19:5000/frontadmine:${DOCKERTAG}+g' ./administrationmanifestfiles/front/deployment-front.yaml

                        echo "⏱ Ajout/Mise à jour de l'annotation redeploy-timestamp..."
                        TIMESTAMP=$(date +%Y%m%d-%H%M%S)
                        grep -q "redeploy-timestamp" ./administrationmanifestfiles/front/deployment-front.yaml && \
                            sed -i "s/redeploy-timestamp:.*/redeploy-timestamp: \\"$TIMESTAMP\\"/" ./administrationmanifestfiles/front/deployment-front.yaml || \
                            sed -i "/labels:/a \ \ \ \ annotations:\\n\ \ \ \ \ \ redeploy-timestamp: \\"$TIMESTAMP\\"" ./administrationmanifestfiles/front/deployment-front.yaml

                        echo "📄 Contenu après modification :"
                        cat ./administrationmanifestfiles/front/deployment-front.yaml

                        echo "📦 Préparation du commit Git..."
                        git add .

                        CHANGES=$(git status --porcelain)
                        if [ -n "$CHANGES" ]; then
                            git commit -m "🚀 Update manifest with image tag ${DOCKERTAG} (Jenkins build)"
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/cnssbrazza.git HEAD:preprod
                        else
                            echo "✅ Aucun changement détecté dans les manifests. Pas de commit."
                        fi
                    '''
                }
            }
        }
    }
}
