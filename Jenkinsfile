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
                sh 'docker build --no-cache -t test .'
                sh 'docker tag test:latest 195778983030.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
            }
        }
        stage ('Uploading to ECR') {
            steps {
                echo "uploading to ECR"
                sh 'docker push 195778983030.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
            }
        }

        stage ('deploying to GKE') {
           steps {
                echo "deploying imges to GKE"
                sh 'kubectl apply -f test-dep.yaml'
                sh 'kubectl set image deployment/httpd-deployment httpd2=saidevops94/repos:latest'
                sh 'kubectl apply -f test-svc.yaml'
                sh 'kubectl rollout restart deployment/httpd-deployment'
                sh 'docker rmi -f $(docker images --filter "dangling=true" -q --no-trunc)'
               
           }
    }  
        
}

}
