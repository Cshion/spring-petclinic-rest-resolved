node {
    stage("Preparation"){
        checkout scm
    }

    stage("Build"){
        steps.sh "chmod +x mvnw"
        steps.sh "./mvnw test-compile"
    }

    stage("Unit Test"){
        steps.sh "./mvnw test"
        steps.junit keepLongStdio: true, testResults: 'target/surefire-reports/TEST-*.xml'
        steps.jacoco execPattern: 'target/**.exec' 
    }
}
