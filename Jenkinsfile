pipeline {
    agent any
    tools {
        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        jdk "1.8"
		maven "3.5.4"

    }
    environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus2"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "3.17.81.137:8081/nexus"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "myrepo"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus1234"
		GROUP_ID = "demo1"
		ARTIFACT_ID = "myarti"
		FILE = "/var/lib/jenkins/workspace/Java/NumberGenerator/target/NumberGenerator-1.0-SNAPSHOT.jar"
		TYPE = "jar"
    }
    stages {
        stage("clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/Ramanjulur/aug-maven.git';
                }
            }
        }
        stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh 'mvn -f NumberGenerator/pom.xml install'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f NumberGenerator/pom.xml test'
            }
            
        }
		stage('Deploy') {
            steps {
			   //sh 'cp -r /var/lib/jenkins/workspace/Java/NumberGenerator/target/NumberGenerator-1.0-SNAPSHOT.jar /opt/'
               sh 'scp -r /var/lib/jenkins/workspace/Java/NumberGenerator/target/NumberGenerator-1.0-SNAPSHOT.jar agent:/var/www/html/'
            }
            
        }

        stage("publish to nexus") {
            steps {
               
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: GROUP_ID,
                            version: NEXUS_VERSION,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: ARTIFACT_ID,
                                classifier: '',
                                file: FILE,
                                type: TYPE],
                                
                            ]
                        );
                    } 
                }
            }
        }
    
