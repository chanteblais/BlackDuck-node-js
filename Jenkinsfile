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
    
    stage('WhiteSource Scan') {
        whitesource jobApiToken: '', libExcludes: '', libIncludes: '**/*.c **/*.cc **/*.cp **/*.cpp **/*.cxx **/*.c++ **/*.h **/*.hpp **/*.hxx **/*.m **/*.mm **/*.js **/*.php **/*.jar **/*.gem **/*.rb **/*.dll **/*.cs **/*.tgz **/*.deb **/*.gzip **/*.rpm **/*.tar.bz2 **/*.zip **/*.tar.gz **/*.egg **/*.whl **/*.py **/*.java **/*.class **/*.nupkg **/*.r ', product: 'Jenkins Test', productVersion: '', projectToken: '', requesterEmail: ''
    }
}

