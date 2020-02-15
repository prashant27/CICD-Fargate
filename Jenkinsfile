node {
    def app

    /*
    stage('Clone-repository') {

        //checkout scm
    }

    stage('Build-image') {

        //app = docker.build("hello-world")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }*/

    stage('Push-image') {
        /*push docker images*/
                //cleanup current user docker credentials
        sh 'rm  ~/.dockercfg || true'
        sh 'rm ~/.docker/config.json || true'
        
        docker.withRegistry('https://440535814002.dkr.ecr.us-east-1.amazonaws.com/fargate', 'ecr:us-east-1:demo-ecr-credential') {
            //app.push("${env.BUILD_NUMBER}")
            docker.image('hello-world').push('latest')
            }   
        

    }

    stage('create-stack'){
        // call cloudformatio template
        /*
        sh """
        $ export AWS_ACCESS_KEY_ID=?
        $ export AWS_SECRET_ACCESS_KEY=?
        $ export AWS_DEFAULT_REGION=us-east-1
        aws cloudformation create-stack --stack-name fargate-vpc --template-body file://stack.yml --region 'us-east-1'
        """
        */
    }

}
