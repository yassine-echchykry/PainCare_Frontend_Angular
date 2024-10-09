pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/yassine-echchykry/PainCare_Frontend_Angular.git', branch: 'main'
            }
        }

        stage('Install Node.js') {
            steps {
                echo 'npm install'
            }
        }
        
        //stage('Build Angular') {
          //  steps {
            //    script {
              //      echo 'npm run build --prod'
                //}
           // }
        //}

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=team7_project \
                            -Dsonar.host.url=https://29de-105-73-96-62.ngrok-free.app \
                            -Dsonar.login=sqa_c71fea0a0f2dc3a496870d130125c68309512de4 \
                            -Dsonar.sources=src \
                            -Dsonar.exclusions="**/node_modules/**"
                        """
                    }
                }
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('SonarQube') {
        //             sh 'docker run --network=host -e SONAR_HOST_URL="http://127.0.0.1:9000" -v "$PWD:/usr/src" sonarsource/sonar-scanner-cli'
        //         }
        //     }
        // }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        // stage('Email Sent') {
        //     steps{
        //         sh 'swaks --to ismailtelhouni123@gmail.com \
        //             --from "chakra.hs.business@gmail.com" \
        //             --server "smtp.gmail.com" \
        //             --port "587" \
        //             --auth PLAIN \
        //             --auth-user "chakra.hs.business@gmail.com" \
        //             --auth-password "pnuw lgzu ofkv oyoq" \
        //             --helo "localhost" \
        //             --tls \
        //             --data "Subject: Sonar Subject Test\n\nSalam Ismail from CLI"'
        //     }
        // }
        // stage('Email Sent') {
        //     steps{
                
        //     }
        // }
    }
    post {
        always {
            echo "Analyse terminée, vérifiez SonarQube pour les résultats."
        }

        failure {
            script {
                emailext(
                    subject: "Pipeline Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> a échoué à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: red;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'ismailtelhouni123@gmail.com', // Remplacez par les adresses souhaitées
                    from:"chakra.hs.business@gmail.com",
                    replyTo:"chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }

        success {
            script {
                emailext(
                    subject: "Pipeline Succeeded: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> s'est terminé avec succès à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: green;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'ismailtelhouni123@gmail.com', // Remplacez par les adresses souhaitées
                    from:"chakra.hs.business@gmail.com",
                    replyTo:"chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }
    }
}
