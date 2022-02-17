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
