pipeline {
    agent any
    
    environment {
        JAVA_HOME = tool 'JDK11' // Assuming JDK 11 is configured in Jenkins tools
        MAVEN_HOME = tool 'Maven' // Assuming Maven is configured in Jenkins tools
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/your-repo.git'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean package' // Build the Maven project
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh '${MAVEN_HOME}/bin/mvn sonar:sonar' // Run SonarQube analysis
                }
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = findFiles(glob: '**/target/*.war').get(0)
                    sh 'curl -T ${warFile} http://username:password@tomcat-server:8080/manager/text/deploy?path=/context-path&update=true'
                }
            }
        }
    }
    
    post {
        always {
            // Clean up steps can be added here
        }
    }
}

