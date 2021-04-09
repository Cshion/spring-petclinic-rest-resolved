node {
    stage("Preparation"){
        checkout scm
    }

    stage("Build"){
        steps.sh "chmod +x mvnw"
        //steps.sh "./mvnw test-compile"
    }

    stage("Unit Test"){
        //steps.sh "./mvnw test"
        //steps.junit keepLongStdio: true, testResults: 'target/surefire-reports/TEST-*.xml'
        //steps.jacoco execPattern: 'target/**.exec' 
    }
    
    stage("Build & Push Docker Image"){
        withCredentials([usernamePassword(credentialsId: 'aws-credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            def loginCommand = steps.sh(script:"aws ecr get-login --region us-east-1", returnStdout:true).trim()
            def token = loginCommand.split(" ")[5]
            
            withEnv([
                "REGISTRY_PASSWORD=$token",
                "REGISTRY_USERNAME=AWS"
            ]) {
                steps.sh "mvn compile com.google.cloud.tools:jib-maven-plugin:3.0.0:build"
            }
            
        }
    }
    
    stage("Deploy application"){
    
    }
    
    stage("Run performance tests"){
        
    }
    
}
