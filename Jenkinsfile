pipeline {
	agent any

stages{
 //------------------------------------------------
  stage('Generate Local Tag') {
    steps {
        script {
            def shortHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
            def timestamp = new Date().format('yyyyMMddHHmmss')
            env.LOCAL_TAG = "v${timestamp}-${shortHash}"

            echo "Generated Local Tag: ${env.LOCAL_TAG}"
        }
    }
    }

 //------------------------------------------------

    stage('Docker Build & Push') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD_NOE', variable: 'DOCKER_HUB_PASSWORD')]) {
                echo "print tag : $LOCAL_TAG"
                sh 'docker login -u nono84569 -p $DOCKER_HUB_PASSWORD'
                sh 'docker build -t nono84569/nodejsnoe:$LOCAL_TAG .'
                sh 'docker push nono84569/nodejsnoe:$LOCAL_TAG'
            }
        }
    }

 //------------------------------------------------

    stage(' Deployment docker ') {
        steps {
           
                script {
                    sh 'sudo docker ps -a --filter "name=nono84569" --format "{{.ID}}" |sudo  xargs -r docker stop'
                    sh 'sudo docker ps -a --filter "name=nono84569" --format "{{.ID}}" |sudo  xargs -r docker rm' 
                    sh 'docker run -d  -p 3350:3000 --name nono84569$LOCAL_TAG nono84569/nodejsnoe:$LOCAL_TAG'
                }
           
        }
    }
//------------------------------------------------
}

}
