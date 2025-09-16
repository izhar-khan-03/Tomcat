pipeline {
    agent any
    
    tools {
        jdk 'jdk-21'
        maven 'Maven3'
    }
    
    environment {
        // Define environment variables for Tomcat
        WAR_FILE = 'target/231422exp5.war' // Path to the generated WAR file
        TOMCAT_URL = 'http://localhost:9090' // Tomcat server URL
        TOMCAT_USER = 'tomcatuser' // Updated Tomcat Manager username from tomcat-users.xml
        TOMCAT_PASSWORD = 'yourpassword' // Updated Tomcat Manager password from tomcat-users.xml
    }
    
    stages {
        stage('Clean Project') {
            steps {
                bat "mvn clean"
            }
        }

        stage('Build Project') {
            steps {
                bat "mvn package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFilePath = "${WORKSPACE}/${WAR_FILE}"
                    echo "WAR file path: ${warFilePath}"
                    
                    // Check if the WAR file exists before deploying
                    if (fileExists(warFilePath)) {
                        echo 'WAR file found, proceeding with deployment...'
                        
                        // Deploy the WAR file to Tomcat using curl and Tomcat Manager API
                        bat """
                            curl --upload-file "${warFilePath}" ^
                            --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} ^
                            "${TOMCAT_URL}/manager/text/deploy?path=/231422exp5&update=true"
                        """
                    } else {
                        error('WAR file not found! Build might have failed.')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
    }
}