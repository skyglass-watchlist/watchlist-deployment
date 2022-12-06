def region = 'eu-central-1'
def accounts = [master:'production', preprod:'staging', develop:'sandbox']

node('master'){
    stage('Checkout'){
        checkout scm
    }

    stage('Authentication'){
        sh "aws eks update-kubeconfig --name ${accounts[env.BRANCH_NAME]} --region ${region}"
    }

    stage('Deploy'){
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
            sh('kubectl apply -f kubectl/deployments/')
            sh ('kubectl apply -f kubectl/services/')
		  }
	   }        
    }
}