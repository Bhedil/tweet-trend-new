pipeline {
    agent {
        node {
            label 'maven'
        }
    }

// for path environment variables defined
environment {
    // PATH = "/opt/apache-maven-3.9.16/bin:$PATH"
    // Allocates up to 2GB of memory to the Sonar JVM and maven server process
    SONAR_SCANNER_OPTS = '-Xmx2048m'
    MAVEN_OPTS="-Xms512m -Xmx2048m"
}

    stages {
        stage("build"){
            steps {
                echo "---------------- build started -----------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------------- build completed -----------------"
            }
        }

        stage("test"){
            steps{
                echo "---------------- unit test started -----------------"
                sh "mvn surefire-report:report"
                echo "---------------- unit test completed -----------------"
            }
        }

        stage('SonarQube analysis') {
            environment{
                scannerHome = tool 'bhedil-sonar-scanner'
            }
            
            steps {
                withSonarQubeEnv('bhedil-sonar-server') { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        //cannot use this stage because of the current capitalism of sonarqube to create a custom quality gates because it locked by paid features.
        // stage("Quality Gate"){
        //     steps{
        //         script {
        //            timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
        //                 def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
        //                 if (qg.status != 'OK') {
        //                     error "Pipeline aborted due to quality gate failure: ${qg.status}"
        //                 }
        //             } 
        //         }
        //     }
        // }
    }
}