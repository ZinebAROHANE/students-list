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
          //steps{
              //dir("$WORKSPACE/simple_api"){

                 // ++++
                 steps{
                 withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerHubPwd')]) {
                    sh(script: """
                     docker login -u arohanezineb -p ${dockerHubPwd}
                     """)
                       }
                       sh(script: """
	                   docker push arohanezineb/studentlistimage:0.0.1
                      """)
                 }
                 }


               /*  
      stage(' app Deploy'){
        steps{
         //ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
           ansiblePlaybook credentialsId: 'private-key25', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
            }
             }
             */


//} }
               //+++ suite push conteiner

                // script{
                  //  docker.withRegistry('https://index.docker.io/v1/ ', 'dockerhub') {
                   // def image = docker.build('arohanezineb/zineblourizrepo:latest')
                   // image.push()
                    
                   // }
             // }
              //}
        //  }



// ================================================= UNTIIL NOW WORKING!!=======================================================================

  

         /*stage('Remove Previous Container'){
            steps{
	         try{
	         	def dockerRm = 'docker rm -f zineblourizrepo'
		         sshagent(['docker-dev']) {
			      sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.91.226 ${dockerRm}"
	            	}
	               }catch(error){
		
	               }
                }
               }

*/
         stage('app Deploy') {
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





