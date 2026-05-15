pipeline {
    agent any
    tools {
        maven 'maven.15'
    }

    environment {
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        NEXUS_REPO_URL = 'http://16.112.31.119:8081/repository/grithikalabs-central-repo/'
        GROUP_ID = 'com.evolve'
        ARTIFACT_ID = 'evolve-technologies'
        VERSION = '2.0'
        PACKAGING = 'war'
        FINAL_NAME = 'evolve-technologies'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/umamahesh571/jenkins-nexus-integration-.git', branch: 'main'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn  package -DskipTests'
            }
        }

        stage('Upload WAR to Nexus') {
            steps {
                sh '''
                    FILE_PATH=target/${FINAL_NAME}.${PACKAGING}
                    GROUP_PATH=$(echo ${GROUP_ID} | tr '.' '/')
                    UPLOAD_URL=${NEXUS_REPO_URL}/${GROUP_PATH}/${ARTIFACT_ID}/${VERSION}/${ARTIFACT_ID}-${VERSION}.${PACKAGING}
                    
                    echo "Uploading WAR to: $UPLOAD_URL"
                    
                    curl -v -u ${NEXUS_USER}:${NEXUS_PASS} --upload-file $FILE_PATH $UPLOAD_URL
                '''
            }
        }
    }

    post {
        success {
            echo '✅ WAR file built and uploaded to Nexus successfully.'
        }
        failure {
            echo '❌ Failed to build or upload the WAR file to Nexus.'
        }
    }
}
