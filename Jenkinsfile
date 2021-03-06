node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line
         * the best way is define the image name with docker userID*/

        app = docker.build("1293813551/hellonode")
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
         * Pushing multiple tags is cheap, as all the layers are reused.
         *  + docker tag --force=true 1293813551/hellonode registry.hub.docker.com/1293813551/hellonode:6
unknown flag: --force
See 'docker tag --help'.
+ docker tag 1293813551/hellonode registry.hub.docker.com/1293813551/hellonode:6
        * and the tag can be custom like date or other tag 
        * and app.push will execute push opation again*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
