pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: """
               cd simple_api/
               docker build -t arohanezineb/studentlistimage:0.0.1 .
               cd ..
            """)
         }

   }
      stage('Push container'){
          steps{
              dir("$WORKSPACE/simple_api"){

                 // ++++
                 withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerHubPwd')]) {
                     sh "docker login -u arohanezineb -p ${dockerHubPwd}"
                       }
	                   sh 'docker push arohanezineb/zineblourizrepo:latest'
               //+++
                // script{
                  //  docker.withRegistry('https://index.docker.io/v1/ ', 'dockerhub') {
                   // def image = docker.build('arohanezineb/zineblourizrepo:latest')
                   // image.push()
                    
                   // }
             // }
              }
          }
}

stage('Remove Previous Container'){
	try{
		def dockerRm = 'docker rm -f zineblourizrepo'
		sshagent(['docker-dev']) {
			sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.85.84 ${dockerRm}"
		}
	}catch(error){
		//  do nothing if there is an exception
	}
 }


         stage('Run container on app server') {
            steps{
                dir("$WORKSPACE/simple_api"){
                 script{
                  def dockerRun = 'docker run -d -p 5000:5000 --name simple_api arohanezineb/zineblourizrepo:latest'

                  sshagent(['dev-server']) {
                   sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.85.84 ${dockerRun}"
                  }
                  }
               }
          }  

   }
   
}
}
/*stage(' app Deploy'){
      steps{
  ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
}
   }*/




