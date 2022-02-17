#!groovy

import groovy.json.JsonSlurperClassic
node {
    def SFDCTREE = false
    def ACT_VERSION='1.314'
    def ACT_PACKAGEID='04t5G000003vfR5'					   
    def GUSWORKITEMS=' W-001414'
    def GUSBUILD='Build AstrosCourseTrackerR.3.10.0'
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    println 'Checked out Source'
    //check packages
   
	    
        if (env.BRANCH_NAME == 'master') {
            stage('Build Master') {
                echo 'Building sfdx master...'
            }
        }
        try {   
		
        if (env.BRANCH_NAME == 'ActCI-Dev') {
	    SFDCTREE = true	
	    notifyBuild('STARTED',ACT_VERSION,ACT_PACKAGEID,GUSWORKITEMS)

            stage('Build Dev Sandbox via sfdx') {
                
               
                if (isUnix()) {
                    echo "isUnix ..................."
		  // rmsg = sh returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u   -w 15"
		   rmsg = sh returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
	
                  
                } else {
                    println 'Not Unix .......................'
		    //msg = bat  returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s   -w 15"
                   // rmsg = bat returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                }
            } //end stage
	
        }//end if dev
	if (env.BRANCH_NAME == 'ActCI-Qa') {
	    SFDCTREE = true
            notifyBuild('STARTED',ACT_VERSION,ACT_PACKAGEID,GUSWORKITEMS)

            stage('Build qa Sandbox Step  ') {
                println "Build Qa Sandbox Step"
		
                 if (isUnix()) {
                    echo "isUnix ..................."
	           // rmsg = sh returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u  -w 15"
	            rmsg = sh returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                } else {
                    println 'Not Unix .......................'
                    rmsg = bat returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u  -w 15"
                    rmsg = bat returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                }
            } //end stage
	  
	
        }//end if qa
	 if (env.BRANCH_NAME == 'ActCI-Uat') {
            SFDCTREE = true
            notifyBuild('STARTED',ACT_VERSION,ACT_PACKAGEID,GUSWORKITEMS)
            stage('Build Uat Sandbox via sfdx') {
                
 
                if (isUnix()) {
                    echo "isUnix ..................."
	          //  rmsg = sh returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u   -w 15"
	            rmsg = sh returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                } else {
                    println 'Not Unix .......................'
                   // rmsg = bat returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u -w 15"
                   // rmsg = bat returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                }
            } //end stage
	
        }//end if uat   
	  if (env.BRANCH_NAME == 'ActCI-test') {
            SFDCTREE = true
            notifyBuild('STARTED',ACT_VERSION,ACT_PACKAGEID,GUSWORKITEMS)
            stage('Build Prod Sandbox via sfdx') {
                
                  if (isUnix()) {
                    echo "isUnix ..................."
	         // rmsg = sh returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -s AdminsOnly -u   -w 15"
	         rmsg = sh returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
			  
	        //  rmsg = sh returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -u jenkins@readiness.salesforce.com.vcd  -w 15"
	       //   rmsg = sh returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u jenkins@readiness.salesforce.com.vcd"
                } else {

                    println 'Not Unix .......................'
                  //  rmsg = bat returnStdout: true, script: "sfdx force:package:install  -p ${ACT_PACKAGEID} -u   -w 15"
                  //  rmsg = bat returnStdout: true, script: "sfdx  force:source:deploy -x manifest/package.xml -u "
                }
            } //end stage
	
        }//end if test   
	    } catch (e) {
            // If there was an exception thrown, the build failed
             currentBuild.result = "FAILED"
               throw e
             } finally {
              // Success or failure, always send notification for valid branches
		
			   
                   switch (SFDCTREE) {
                    case true:
		       notifyBuild(currentBuild.result,ACT_VERSION,ACT_PACKAGEID,GUSWORKITEMS)
                       break
                    case false:
		       println("Branch  ${env.BRANCH_NAME} Not Found in Build Flow ")
                       break
             
                   
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
 slackSend (color: colorCode, message: summary)
    
}
