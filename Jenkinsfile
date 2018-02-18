node ('node-0.4-grovey'){
    
 notify('Started')    
 
 stage ('SCM_Checkout') {
    checkout([$class: 'GitSCM', 
        branches: [[name: '*/master']], 
        doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
        userRemoteConfigs: [[url: 'https://github.com/ganeshhp/Maven-petclinic-project.git']]])
 }
 stage ('Build_Test and Package') {
    sh 'mvn clean verify package'
//    junit 'target/surefire-reports/TEST*.xml'
 }
 
 stage ('Archive and Notify') {
    publishHTML(target: [allowMissing: true, 
                 alwaysLinkToLastBuild: false, 
                 keepAll: true, 
                 reportDir: 'target/site/jacoco', 
                 reportFiles: 'index.html', 
                 reportName: 'HTML Report', 
                 reportTitles: 'Code Coverage-Report'])

    archiveArtifacts 'target/*.war' 
    step([$class: 'Mailer', 
        notifyEveryUnstableBuild: true, 
        recipients: 'cc:akd2patil@outlook.com', 
        sendToIndividuals: false])
 }
 notify ('Waiting for Deployment')
 
 input 'Deploy to Staging?'
 
 stage ('Deploy to AppServer') {
 sh 'cp target/petclinic.war /home/akshay1306patil/apache-tomcat-9.0.0.M10/webapps'
 sh 'sudo /home/akshay1306patil/apache-tomcat-9.0.0.M10/bin/shutdown.sh'
 sh 'sudo /home/akshay1306patil/apache-tomcat-9.0.0.M10/bin/startup.sh'
 }
 
  notify('Completed')  
}

 def notify(status) {
  mail (
        body:"""${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':
                 Check console output at, 
                 href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]""",
        cc: 'support@automationfactory.in', 
        subject: """JenkinsNotification: ${status}:""", 
        to: 'akd2patil@outlook.com'  
       ) 
 }
