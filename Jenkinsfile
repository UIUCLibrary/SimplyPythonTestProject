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
            agent {
                label 'Windows'
            }

            steps {
                unstash "Source"
                bat "${env.TOX}  --skip-missing-interpreters"


            }
        }
        stage("Packaging Windows wheel") {
            agent {
                label 'Windows'
            }
            steps {
                deleteDir()
                unstash "Source"
                bat "${env.PYTHON3} setup.py bdist_wheel --universal"
                stash includes: 'dist/*.whl', name: "Windows_Wheel"
            }

        }
        stage("Packaging Source") {
            agent any

            steps {
                deleteDir()
                unstash "Source"
                sh "${env.PYTHON3} setup.py sdist"
                stash includes: 'dist/*.*', name: "Source_Dist"
            }

        }

        stage("Archiving") {
            agent any

            steps {
                deleteDir()
                echo "Packaging"
                unstash "Windows_Wheel"
                unstash "Source_Dist"
                archiveArtifacts artifacts: "**", fingerprint: true
            }
        }

    }
}
