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


               
      stage(' app Deploy'){
        steps{
           ansiblePlaybook credentialsId: 'private-key25', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
            }
             }
             


} 
}
     



