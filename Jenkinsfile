// define the docker containers we want here:
podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'git', image: 'alpine/git', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'gradle', image: 'gradle', command: 'cat', ttyEnabled: true),
  ]
  )
{
    // automatically label the pod - see https://plugins.jenkins.io/kubernetes/ for more info
    node(POD_LABEL) {

        // cloen a git repo that contains a gradle project
        stage('Clone git repository') {
            // run the following commands in the git container
            container('git') {
                sh 'git clone -b master https://github.com/AutomatedIT/springbootjenkinspipelinedemo.git'
                // show the cloned project files, just for info
                sh 'find .'
            }
        }

        // run the example gradle build
        stage('Gradle Build') {
            // insoide the gradle docker image now
            container('gradle') {
                    // run gradle build
                    sh 'cd springbootjenkinspipelinedemo; ./gradlew clean build'
                    // a little more info on the output
                    sh 'ls -al springbootjenkinspipelinedemo/build/libs/'
                    // finally, archive the built application
                    archiveArtifacts artifacts: 'springbootjenkinspipelinedemo/build/libs/**/*.jar', fingerprint: true
            }
        }

    }
}
