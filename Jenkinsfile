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
                bat "${env.TOX}  --skip-missing-interpreters"


            }
        }
        stage("Packaging Windows wheel") {
            agent {
                label 'Windows'
            }
            steps{
                bat "${env.PYTHON3} setup.py bdist_wheel --universal"

            }

        }

    }
}
