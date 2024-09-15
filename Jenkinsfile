pipeline {
    agent {
        label 'docker-agent-alpine'
    }

    tools{
         maven 'maven_3.9.9'
    }    
    
    stages {
        stage('Build') { 
            steps {
                sh "echo PATH: $PATH"
                sh "mvn --version"
                sh "echo start building with mvn, skipping test"
                sh "mvn -B -DskipTests -Denforcer.skip=true clean package"
                
            }
        }

         stage('Unit Test') {
            steps {
                script {
                    sh 'echo Running test'
                    sh "mvn test"
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/my-app-1.0-SNAPSHOT.jar',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true
        }

        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
        }
    }
}