node {
    stage("Preparation"){
        checkout scm
    }

    stage("Build"){
        //sh "chmod +x mvnw"
        //sh "./mvnw test-compile"
    }

    stage("Unit Test"){
        //sh "./mvnw test"
        //junit keepLongStdio: true, testResults: 'target/surefire-reports/TEST-*.xml'
        //jacoco execPattern: 'target/**.exec' 
    }
    
    stage("Build & Push Docker Image"){
        withCredentials([usernamePassword(credentialsId: 'aws-credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            def loginCommand = steps.sh(script:"aws ecr get-login --region us-east-1", returnStdout:true).trim()
            def token = loginCommand.split(" ")[5]
            
            withEnv([
                "REGISTRY_PASSWORD=$token",
                "REGISTRY_USERNAME=AWS"
            ]) {
                //sh "mvn compile com.google.cloud.tools:jib-maven-plugin:3.0.0:build"
            }
            
        }
    }
    
    stage("Deploy application"){
        withCredentials([usernamePassword(credentialsId: 'aws-credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            def loginCommand = steps.sh(script:"aws ecr get-login --region us-east-1", returnStdout:true).trim()
            def token = loginCommand.split(" ")[5]
            def user = loginCommand.split(" ")[3]
            /*
            sshagent(['ee6265b6-c407-4920-8f04-3bc930f4e518']) {
                sh """ssh -o StrictHostKeyChecking=no ubuntu@54.86.48.102 << EOF
                    echo 'Login docker'
                    docker login -u $user -p $token 579931652533.dkr.ecr.us-east-1.amazonaws.com
                    echo 'Docker pull'
                    docker pull 579931652533.dkr.ecr.us-east-1.amazonaws.com/demo/spring-petclinic-rest:latest
                    echo 'Docker stop'
                    docker stop petclinic || echo 'Stopped'
                    echo 'Docker RM'
                    docker rm petclinic || echo 'Removed'
                    echo 'Docker RUN'
                    docker run --name petclinic -d -p 9966:9966 579931652533.dkr.ecr.us-east-1.amazonaws.com/demo/spring-petclinic-rest:latest
                    echo 'Docker Logout'
                    docker logout 579931652533.dkr.ecr.us-east-1.amazonaws.com
                """
            }
            */
        }

    }
    
    stage("Run performance tests"){
        sh "jmeter -j jmeter.save.saveservice.output_format=xml -n -t jmeter-petclinic-server.jmx -l jenkins.io.report.jtl"
    }
    
}
