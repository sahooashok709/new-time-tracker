pipeline{
    agent any
    stages{
        stage("git checkout"){
            steps{
              git branch: 'master' url: 'https://github.com/sahooashok709/new-time-tracker.git'
            }
        }
        stage("build in maven"){
            steps{
                sh "mvn clean package"
            }
        }
        stage('SonarAnalysis') {
            steps {
                withSonarQubeENV('sonarqube_mine') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
       stage("deploy in tomcat"){
            steps{
                sshagent(['jenkins_declarative']) {
                sh """
                    scp -o StrictHostKeyChecking=no $JENKINS_HOME\workspace\jenkins_declarative_pipeline\web\target\time-tracker-web-0.5.0-SNAPSHOT.war  ec2-user@172.31.45.158:/opt/tomcat8/webapps/
                    ssh ec2-user@172.31.45.158 /opt/tomcat8/bin/shutdown.sh
                    ssh ec2-user@172.31.45.158 /opt/tomcat8/bin/startup.sh
                   """ 
                }
            }
        } 
      
    }
}
