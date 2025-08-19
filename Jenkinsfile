node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update Manifest & Push') {
        script {
            // On récupère directement les paramètres du job
            def manifestPath = MANIFEST_PATH

            withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIAL', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh "git config user.email brahimcnss2025@gmail.com"
                sh "git config user.name brahimcnss"

                echo "📦 Mise à jour du manifest pour ${SERVICE}"
                echo "➡️ Fichier manifest : ${manifestPath}"

                // Mise à jour de l'image dans le manifest
                sh """
                    sed -i 's+192.168.2.19:5000/${SERVICE}.*+192.168.2.19:5000/${SERVICE}:${DOCKERTAG}+g' ${manifestPath}
                """

                // Vérification
                sh "cat ${manifestPath}"

                // Commit si nécessaire
                sh "git add ${manifestPath}"
                def changes = sh(script: "git status --porcelain", returnStdout: true).trim()

                if (changes) {
                    sh "git commit -m 'Jenkins: maj manifest ${SERVICE} build ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/cnssbrazza.git HEAD:preprod"
                } else {
                    echo "✅ Aucun changement détecté pour ${SERVICE}"
                }
            }
        }
    }
}
