pipeline {
    agent any

    triggers {
        pollSCM('*/5 * * * *')
    }

    stages {

        stage('System info') {
            steps {
                echo "Print all environment variables for problem solving"
                sh 'printenv'
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Convert to HTML5') {
            steps {
                sh './gradlew asciidoctor'
            }
        }

        stage('Publish to Confluence') {
            environment {
                CONFLUENCE_CREDENTIALS = credentials('credentials-confluence-api-publish')
            }
            steps {
                sh 'doctoolchain \
                        . \
                        publishToConfluence \
                        -PmainConfigFile=PublishToConfluenceConfig.groovy \
                        --no-daemon'
            }
        }
    }
}