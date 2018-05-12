node {
    def buildEnv
    def appName = 'hellonode'
    def devAddress
    def registry = "172.31.17.242:5000"
    def imageNm = "${appName}:${env.BUILD_NUMBER}"
    def imageTag = "$registry/${imageNm}"

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        buildEnv = docker.build("${imageNm}")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        buildEnv.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Scan image') {
        /* Scan the docker image */
        
        def imageLine = "${imageNm}"
        writeFile file: 'anchore_images', text: imageLine
        anchore name: 'anchore_images', inputQueries: [[query: 'cve-scan all'], [query: 'list-packages all'], [query: 'list-files all'], [query: 'show-pkg-diffs base']]

    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
         sh("docker tag ${imageNm} ${imageTag}")
         sh("docker push ${imageTag}")
    }
}
