#!groovy

import groovy.json.JsonSlurperClassic
node {
	
    def SFDCTREE = false

	
    def ACT_VERSION='1.296.0'
    def ACT_PACKAGEID='04t5G000003vf1z'
    def GUSWORKITEMS='W-000947'
    def GUSBUILD='Build AstrosCourseTrackerR.3.6.0'
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    println 'Checked out Source'
    //check packages
   
	    
        if (env.BRANCH_NAME == 'master') {
            stage('Build Master') {
                echo 'Building sfdx master'
            }
        }
        
    
}//end node

def notifyBuild(String buildStatus = 'STARTED',String version,String id,String gus) {
	
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL}) Appiphony Package ${version} ID: ${id} ACT 20.0 GUS Items: ${gus} "
  def details = """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

 echo "${summary}"
 //slackSend (color: colorCode, message: summary)
    
}
