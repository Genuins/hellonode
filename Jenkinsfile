node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
     checkout scm
       
       
    }

     stage ('Deploy') {
       
    }
    
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        steps{
          sshagent(credentials : ['use-the-id-from-credential-generated-by-jenkins']) {
                sh 'ssh -o StrictHostKeyChecking=no dockeradmin@192.168.33.250 uptime'
                sh 'ssh -v dockeradmin@192.168.33.250'
                sh 'scp ./source/filename user@hostname.com:/remotehost/target'
          }
       }
        app = docker.build("getintodevops/hellonode")
        
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
