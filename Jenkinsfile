node{
    def mavenHome = tool name: 'Maven3.9.2'
  stage('1CloneCode'){
    git "https://github.com/mc-devops-class/visa-app"
    //sh "git clone https://github.com/mc-devops-class/visa-app"
  }
stage('2Test&Build'){
    sh "${mavenHome}/bin/mvn clean package"
}
  stage('3CodeQuality'){
      sh "${mavenHome}/bin/mvn clean sonar:sonar"
  }
  stage('4uploadArtifacts'){
      sh "${mavenHome}/bin/mvn deploy"
  }
  stage('5Deploy2UAT'){
      sh "echo 'deploy to UAT' "
      deploy adapters: [tomcat9(credentialsId: '81022a9f-075b-48f2-88c8-51e6fbaf5a36', path: '', url: 'http://3.101.143.199:8080/')], contextPath: null, war: 'target/*war'
  }
  stage('6ApprovalGate'){
      sh "echo 'ready for review' "
    timeout(time:5, unit:'DAYS') {
    input message: 'Application ready for deployment, Please review and approve'
      }
  }
  stage('7deploy2Prod'){
      deploy adapters: [tomcat9(credentialsId: '81022a9f-075b-48f2-88c8-51e6fbaf5a36', path: '', url: 'http://3.101.143.199:8080/')], contextPath: null, war: 'target/*war'
  }
  stage('8emailNotification'){
      emailext body: '''successfully deployed the clients project online

Thanks team
Everness Tech''', recipientProviders: [buildUser(), developers(), contributor()], subject: 'Deployment status', to: 'visa-prob-job@gmail.com'
  }
}
