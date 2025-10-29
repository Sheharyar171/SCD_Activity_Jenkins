pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build from')
        string(name: 'STUDENT_NAME', defaultValue: 'Syed Sheharyar Ali') // ← Your name here
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run Jest tests after build')
    }
    environment {
        APP_VERSION = "1.0."
        MAINTAINER = "Student"
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: "
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                echo "Installing required packages..."
                bat 'npm install'
            }
        }
        stage('Build') {
            steps {
                echo "Building version  for  environment"
                bat '''
                    echo Simulating build process...
                    if not exist build mkdir build
                    copy src\\*.js build
                    copy *.js build 2>nul || echo "No root JS files to copy"
                    echo Build completed successfully!
                    echo App version: %APP_VERSION% > build\\version.txt
                '''
            }
        }
        stage('Test') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                echo "Running Jest tests..."
                bat 'npm test'
            }
        }
        stage('Package') {
            steps {
                echo "Creating zip archive for version "
                bat 'powershell Compress-Archive -Path build\\* -DestinationPath build_%APP_VERSION%.zip'
            }
        }
        stage('Deploy (Simulation)') {
            steps {
                echo "Simulating deployment of version  to "
            }
        }
    }
    post {
        always {
            echo "Cleaning up workspace..."
            deleteDir()
        }
        success {
            echo "Pipeline succeeded! Version  built and tested."
        }
        failure {
            echo "Pipeline failed! Check console output for details."
        }
    }
}
