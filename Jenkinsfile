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
            stage ('build') {
                steps {
                    rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "JFROG_CLOUD",
                    releaseRepo: 'medha-libs-release-local',
                    snapshotRepo: 'medha-libs-snapshot-local'
                    )
                }
            }
            stage ('Exec Maven') {
                    steps {
                        rtMavenRun (
                            tool: 'MAVEN', // Tool name from Jenkins configuration
                            pom: 'pom.xml',
                            goals: 'clean install',
                            deployerId: "SPC_DEPLOYER",
                            )
                        }
                }
            stage ('Publish build info') {
                    steps {
                        rtPublishBuildInfo (
                            serverId: "JFROG_CLOUD"
                        )
                    }
                }
            stage ('reporting') {
                steps{
                    junit testResults: '**/target/surefire-reports/TEST-*.xml'
                }
            }    
            }
        }
 