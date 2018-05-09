node {
    def app

    stage ('Checkout') {
      deleteDir()
      sh git clone https://github.com/hgsat123/hellonode.git
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("hgsat123/hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Scan image') {
        /* Scan the docker image.    
        
        def imageLine = IMAGE + ' ' + env.WORKSPACE + '/DockerFile'
        writeFile file: 'anchore_images', text: imageLine
        anchore name: 'anchore_images', policyName: 'anchore_policy', bailOnFail: false, inputQueries: [[query: 'list-packages all'], [query: 'cve-scan all']]

    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
#        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
         docker.withRegistry('172.31.17.242:5000') {
          
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
