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
        SONARSERVER = 'sonarserver'
        SONARSCANNER= 'sonarscanner'
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

        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
              scannerHome = tool 'sonarscanner'
          }

          steps {
             withSonarQubeEnv("${SONARSERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
          }
        }
    }
}