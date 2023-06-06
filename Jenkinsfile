node {
    def app

    stage('Clone Repository') {
      

        checkout scm
    }

    stage('Build Image') {
  
       app = docker.build("protech07/packages")
    }

    stage('Test Image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updates"
                build job: 'updates', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
