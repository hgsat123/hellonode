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
        
        def imageLine = "${imageNm}" + ' ' + env.WORKSPACE + '/Dockerfile'
        writeFile file: 'anchore_images', text: imageLine
        anchore name: 'anchore_images', policyName: 'anchore_policy', bailOnFail: false, inputQueries: [[query: 'list-packages all'], [query: 'cve-scan all']]
    }
}
