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
               docker build -t arohanezineb/studentlistimage .
               cd ..
            """)
         }

   }
      stage('Push container'){
          steps{
              dir("$WORKSPACE/simple_api"){
                 script{
                    docker.withRegistry('https://index.docker.io/v1/ ', 'dockerhub') {
                    def image = docker.build('arohanezineb/zineblourizrepo:latest')
                    image.push()
                    
                    }
              }
              }
          }
}
stage(' app Deploy'){
      steps{
  ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
}
   }
   }
}





