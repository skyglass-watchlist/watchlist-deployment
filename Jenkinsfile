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
        sh """
            helm dependency update watchlist
            helm upgrade --install watchlist ./watchlist -f values.override.yaml \
                --set metadata.jenkins.buildTag=${env.BUILD_TAG} \
                --set metadata.git.commitId=${getCommitId()}
        """
    }
}

def getCommitId() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}