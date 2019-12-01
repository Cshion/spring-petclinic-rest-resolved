node {
    stage("Build"){
        steps.sh "./mvnw test-compile"
    }

    stage("Unit Test"){
        steps.sh "./mvnw test"
    }
}
