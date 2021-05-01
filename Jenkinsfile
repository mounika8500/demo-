pipeline {
    agent any
         stages{
    
    stage ('git clone') {
            steps {
        echo "code is building"
         git 'https://github.com/umahari/testing.git'
            }
        }

        stage ('Bulding docker docker image') {
            steps {
                echo "build docker image"
                sh 'sudo docker build --no-cache -t test .'
                sh 'sudo docker tag test:latest 195778983030.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
            }
        }
        stage ('Uploading to ECR') {
            steps {
                echo "uploading to ECR"
                sh 'sudo docker push 195778983030.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
            }
        }

        stage ('deploying to EKS') {
           steps {
                echo "deploying imges to EKS"
                sh 'sudo kubectl apply -f test-dep.yaml'
                sh 'sudo kubectl set image deployment/httpd-deployment httpd2=195778983030.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
                sh 'sudo kubectl apply -f test-svc.yaml'
                sh 'sudo kubectl rollout restart deployment/httpd-deployment'
                sh 'sudo docker rmi -f $(docker images --filter "dangling=true" -q --no-trunc)'
               
           }
    }  
        
}

}
