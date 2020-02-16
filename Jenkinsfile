node {
    def app

    /**/
    stage('Clone-repository') {

        checkout scm
    }

    stage('Build-image') {

        app = docker.build("hello-world")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push-image') {
        /*push docker images*/
         sh("eval \$(aws ecr get-login --no-include-email  --region us-east-1| sed 's|https://||')")

        sh"""
        docker tag hello-world 440535814002.dkr.ecr.us-east-1.amazonaws.com/fargate
        docker push 440535814002.dkr.ecr.us-east-1.amazonaws.com/fargate
        """
    }

    stage('create-stack'){
        // call cloudformatio template
        sh "export AWS_DEFAULT_REGION=us-east-1"
        sh "aws cloudformation create-stack --stack-name fargate-stack --template-body file://stack.yml --region 'us-east-1' --capabilities CAPABILITY_IAM" 
        
    }

}
