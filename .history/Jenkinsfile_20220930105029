pipeline{

    agent any 

    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment{

        SNAP_REPO = "release-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "admin"
        RELEASE_REPO = "weekly-release"
        CENTRAL_REPO = "maven-central"
        NEXUSIP = "172.31.23.121"
        NEXUSPORT= "8081"
        NEXUS_GRP_REPO= "project-maven-group"
        NEXUS_LOGIN = "nexuslogin"
    }   

    stages{
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now archiving."
                    archiveArtifacts artifacts: '**/*.war'  // archiving in war format
                }
            }
           
        }
        

        stage('Test'){

            steps {
                sh 'mvn -s settings.xml test'  // Run unit tests
            }

        }

        stage('Checkstyle Analysis'){

            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'  // Code analysis tool checkstyle to check Vulnerabilities 
            }

        }
    }
}