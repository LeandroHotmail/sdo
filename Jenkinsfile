#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=$BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=$HUB_ORG_DH
    def SFDC_HOST=$SFDC_HOST_DH
    def JWT_KEY_CRED_ID=$JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=$CONNECTED_APP_CONSUMER_KEY_DH

    echo 'KEY IS' 
    echo JWT_KEY_CRED_ID
    echo HUB_ORG
    echo SFDC_HOST
    echo CONNECTED_APP_CONSUMER_KEY
    //def toolbelt = tool 'sfdx'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            if (rc != 0) { error 'hub org authorization failed' }

			echo rc
			
			// need to pull out assigned username
			rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			  
            echo rmsg
            echo('Hello from a Job DSL script!')
            echo(rmsg)
        }
    }
}