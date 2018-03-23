node (label: 'jbuild01_docker') {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        
        app = docker.build("chanteblais/node-web-app")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
            sh 'echo "Jenkins URL: " + $JENKINS_URL'
            sh 'echo "Job Name: " + $JOB_NAME'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'cbDockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
    stage('BlackDuck Scan') {
        hub_scan bomUpdateMaxiumWaitTime: '5', dryRun: false, hubProjectName: 'Jenkins Test CB', hubProjectVersion: '1.0', hubVersionDist: 'OPENSOURCE', hubVersionPhase: 'DEVELOPMENT', scanMemory: '4096', scans: [[scanTarget: '']], shouldGenerateHubReport: true, codeLocationName: 'http://jenkins01.core.rcsops.com/-Black Duck Node-JS Test'
        hub_scan_failure failBuildForPolicyViolation: false
    }
}

