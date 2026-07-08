pipeline {
    agent {
        node {
            label 'maven'
        }
    }

// for path environment variables defined
// environment {
//     PATH = "/opt/apache-maven-3.9.16/bin:$PATH"
// }

    stages {
        stage("build"){
            steps {
                sh 'mvn clean deploy'
            }
        }
    }
}
