node {
    /* Requires the Docker Pipeline plugin to be installed */
    docker.image('node:22.17.0-alpine3.22').inside {
        stage('Test') {
            sh 'node --eval "console.log(process.platform,process.env.CI)"'
        }
    }
}