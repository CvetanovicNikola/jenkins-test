pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/CvetanovicNikola/jenkins-test.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                    
                    emailext(
                         to: 'cvetanovic.nikola@gmail.com',
                        from: 'cvetanovic.nikola@gmail.com',
                        replyTo: 'cvetanovic.nikola@gmail.com',
                        subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """\
                            Hello,

                            The Jenkins build ${env.JOB_NAME} #${env.BUILD_NUMBER} was successful.

                            View the build: ${env.BUILD_URL}

                            Best,
                            Krle
                            """,
                        mimeType: 'text/plain'
                    )
                }
                failure {
                    emailext(
                        subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The build has failed.\n\nCheck console: ${env.BUILD_URL}",
                        to: "cvetanovic.nikola@gmail.com"
                    )
                }
            }
        }
    }
}
