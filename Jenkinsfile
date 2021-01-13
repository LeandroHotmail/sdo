#!groovy
import groovy.json.JsonSlurperClassic
import com.cloudbees.plugins.credentials.CredentialsProvider;
import com.cloudbees.plugins.credentials.common.StandardUsernamePasswordCredentials;
import com.cloudbees.hudson.plugins.folder.Folder
import com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl
node {

    String BUILD_NUMBER=env.BUILD_NUMBER
    String RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    String SFDC_USERNAME

    String HUB_ORG=env.HUB_ORG_DH
    String SFDC_HOST = env.SFDC_HOST_DH
    String JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    String CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    echo 'KEY IS' 
    echo ${JWT_KEY_CRED_ID}
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    //def toolbelt = tool 'sfdx'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			
			// need to pull out assigned username
			rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			  
            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)
        }
    }
}