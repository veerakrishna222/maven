
node('pro1') {
// Delete the workspace
//deleteDir()
stage('Retrieve source code') {
    checkout scm
    delivery = load 'repository.groovy'
    sh " cd $WORKSPACE;/bin/mkdir Build-${env.BUILD_NUMBER} "
    }
try {
    stage('Maven Build') {
      docker.image('maven:3.5-jdk-8-alpine').inside {
        sh "mvn clean package -Dbuild.number=${BUILD_NUMBER}"
        sh "/bin/mv -f $WORKSPACE/target/*.war $WORKSPACE/Build-${env.BUILD_NUMBER}/vsvyadav_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war"
        sh "/bin/cp -f $WORKSPACE/Build-${env.BUILD_NUMBER}/vsvyadav_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war $WORKSPACE/vsvyadav.war"
       }
    }
   stage('Deploy') {
        sh "/bin/mv -f $WORKSPACE/vsvyadav.war /opt/apache-tomcat-8.5.41/webapps/"
    }
  
   delivery.artifactory()
  }
  catch (e) {
      currentBuild.result = "FAILED"
      throw e
    }
}

