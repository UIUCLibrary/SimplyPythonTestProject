pipeline {
    agent none

    stages {
        stage("Cloning source") {
            agent any

            steps {
                echo "Cleaning up"
                deleteDir()
                echo "Cloning source"
                checkout scm
                stash includes: '**', name: "Source"
            }



        }


        stage("Testing Windows") {
            agent{
                label 'Windows'
            }

            steps{
                unstash "Source"
                bat 'TOX  --skip-missing-interpreters'


            }
        }

    }
}
