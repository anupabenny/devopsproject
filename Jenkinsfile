pipeline {

    agent {

        label "master"
    }

    tools {
        maven "Maven"

    }

    environment {

        NEXUS_VERSION = "nexus3"

        NEXUS_PROTOCOL = "http"

        NEXUS_URL = "localhost:8081"

        NEXUS_REPOSITORY = "maven-nexus-repo"

        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"

    }

    stages {

        stage("Clone code from VCS") {

            steps {

                script {

                    git 'https://github.com/ronneyismyboy/devopsproject.git';

                }

            }

        }

        stage("Maven Build") {

            steps {

                script {

                    sh "mvn package -DskipTests=true"

                }

            }

        }
        
        stage('Sonar Scan'){
            echo "Scanning application for vulnerabilities..."
            sh "mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=3e4dbb6c4204a880c8380ee242d191ca0e401e43"
        }

        stage("Publish to Nexus Repository Manager") {

            steps {

                script {

                    pom = readMavenPom file: "pom.xml";

                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");

                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"

                    artifactPath = filesByGlob[0].path;

                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {

                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        nexusArtifactUploader(

                            nexusVersion: NEXUS_VERSION,

                            protocol: NEXUS_PROTOCOL,

                            nexusUrl: NEXUS_URL,

                            groupId: pom.groupId,

                            version: pom.version,

                            repository: NEXUS_REPOSITORY,

                            credentialsId: NEXUS_CREDENTIAL_ID,

                            artifacts: [

                                [artifactId: pom.artifactId,

                                classifier: '',

                                file: artifactPath,

                                type: pom.packaging],

                                [artifactId: pom.artifactId,

                                classifier: '',

                                file: "pom.xml",

                                type: "pom"]

                            ]

                        );

                    } else {

                        error "*** File: ${artifactPath}, could not be found";

                    }

                }

            }

        }

    }

}
