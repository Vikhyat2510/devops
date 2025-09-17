pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Ensure workspace is clean & fetch latest code
                deleteDir()
                git branch: 'main', url: 'https://github.com/Vikhyat2510/devops.git'
            }
        }

        stage('Debug Backend') {
            steps {
                dir('backend') {
                    sh 'pwd'
                    sh 'ls -la'
                    sh 'cat package.json'
                }
            }
        }

        stage('Backend Install & Test') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    sh 'npm test'   // will run dummy test
                }
            }
        }
    }
}
