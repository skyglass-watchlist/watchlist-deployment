def swarmManager = '10.1.2.104'

node('master'){
    stage('Checkout'){
        checkout scm
    }

     sshagent (credentials: ['swarm-sandbox']){
         stage('Copy'){
            sh "scp -o StrictHostKeyChecking=no docker-compose.yml ec2-user@${swarmManager}:/home/ec2-user"
        }

        stage('Deploy stack'){
            sh "ssh -oStrictHostKeyChecking=no ec2-user@${swarmManager} docker stack deploy --compose-file docker-compose.yml --with-registry-auth watchlist"
        }
     }
}
