node('docker') {

    stage('Git Checkout') {
        git branch: 'master', credentialsId: 'gihub-key', url: 'git@github.com:stuartshay/ImageGallery.API.Client.git'
    }

  
    stage('Build & Deploy Docker') {
          sh '''mv docker/image-gallery-api-client.dockerfile/.dockerignore .dockerignore
          docker build -f docker/image-gallery-api-client.dockerfile/Dockerfile --build-arg BUILD_NUMBER=${BUILD_NUMBER} -t stuartshay/imagegallery-api-client:2.1-build  .'''
          withCredentials([usernamePassword(credentialsId: 'docker-hub-navigatordatastore', usernameVariable: 'DOCKER_HUB_LOGIN', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
            sh "docker login -u ${DOCKER_HUB_LOGIN} -p ${DOCKER_HUB_PASSWORD}"
        }
        sh '''docker push stuartshay/imagegallery-api-client:2.1-build'''
    }
    

    stage('Mail') {
        emailext attachLog: true, body: '', subject: "Jenkins build status - ${currentBuild.fullDisplayName}", to: 'sshay@yahoo.com'
    }

}
