  pipeline {
  agent any
  environment {
        AWS_ACCOUNT = "217034708642"
        MODULE = "lionsbot1"
        ENV = "development"
        CLUSTER= "Lionsbot-EKS"
        NAMESPACE="development"
        REGION="us-east-2"
        PORT="8081"
        API_HOST="http://api.iac.dev.securefund.com"
//217034708642.dkr.ecr.us-east-2.amazonaws.com/lionsbot1
        //697979524560.dkr.ecr.us-east-1.amazonaws.com/backend
    }
  stages {
    stage('Build docker image') {
      steps {
        sh('aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com')
        sh('cd web && docker build \
        --build-arg PORT=${PORT} \
        --build-arg API_HOST=${API_HOST} \
        -t ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/${MODULE}:${BUILD_NUMBER} .')
      }
    }
    stage('push docker image') {
      steps {
        sh('aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com')
        sh('docker push ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/${MODULE}:${BUILD_NUMBER}')
      }
    }
//     stage('helm update') {
//       steps {
//        sh('aws s3 cp s3://toptal-helm-chart ${MODULE} --recursive')
//         sh('aws eks --region ${REGION} update-kubeconfig --name ${CLUSTER}')
//         sh("sed -i -e 's#latest#${BUILD_NUMBER}#' web/helm/values.yaml  && cat web/helm/values.yaml")
//         sh('helm upgrade --install --debug -f web/helm/values.yaml ${ENV}-${MODULE} ./${MODULE} -n ${NAMESPACE}')
        //sh("aws cloudfront create-invalidation --distribution-id E34H5J6GSH4FGP --paths '/*' >> /dev/null")



      }
    }
  }
  post {
    always {
      //Cleanup workspace	
      //docker system prune -af
      cleanWs()
      sh('docker system prune -af')

    }

  }
}
