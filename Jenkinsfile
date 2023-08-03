pipeline {
    agent any
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK-17'
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/RAJAKUMARASWAMI/sprin.git',
                    branch: 'main'
            }
        }
        stage('build and package') {
            steps {
                sh script: '/usr/share/maven3.9/apache-maven-3.9.4/bin/mvn package'
            }
        }
        stage('reporting') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.1.0-SNAPSHOT.jar'
                junit testResults: '*/target/surefire-reports/TEST-.xml'
            }
        }
    }
    post {
        success{
            mail subject: "${JOB_NAME} has completed with success",
                 body: "your project is effective \n Build url ${BUILD_URL}",
                 to: 'hgsdhjyfds@test.com'
        }
        failure{
            mail subject: "${JOB_NAME} has completed with fail",
                 body: "your project is effective \n Build url ${BUILD_URL}",
                 to: 'hgsdhjyfds@test.com'
        }
    }

}