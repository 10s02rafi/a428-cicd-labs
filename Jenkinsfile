

node {
    def dockerImage = 'node:16-buster-slim'
    pipelineTriggers([
        pollSCM('*/2 * * * *')
    ])

    try {
        stage('Build and Test') {
            echo 'Building the project...'
            docker.image(dockerImage).inside("-p 3000:3000") {
                sh 'npm install'
                echo 'Running tests...'
                sh './jenkins/scripts/test.sh'
            }
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        error "Pipeline failed: ${e.message}"
    } finally {
        echo 'Cleaning up...'
        sh "docker system prune -af"
    }
}
