pipeline{
    agent any
    environment{
        DEPLOYMENT_ENVIRONMENT = getEnvName(env.BRANCH_NAME)        
    }
    stages{
        stage("cleaning the workspace"){
            steps{
                cleanWs()
            }
        }
        // stage("Checkout code") {
        //     steps {
        //         git url: 'https://github.com/Thiyagarajan-compunnel/POC-replay-option.git', branch: 'development'
        //     }
        // }
        stage('Configuring appsetting.json'){
            when {
                allOf {
                    expression { environment name: 'DEPLOYMENT_ENVIRONMENT', value: 'dev'}
                    expression { environment name: 'DEPLOYMENT_ENVIRONMENT', value: 'qa'}
                }
            }
            environment{
                STAFFLINEAPIURL = '"http://stafflineapi-beta.compunnel.com/"'
                DATABASECONNECTIONSTRING = '"Data Source=20.235.248.163;Initial Catalog=csg_2001_Staging; MultipleActiveResultSets=True; User ID=csgdbstaging; Password=djj8@s#sjs;Persist Security Info=True;Pooling=Yes;"'
                ISSUER = '"https://dev-api.velocityln.ai/"'
                AKVURL = '"https://vln-key-vault.vault.azure.net/"'
            }
            steps{
                sh """
                echo "${STAFFLINEAPIURL}"
                echo "${ISSUER}"
                echo "${AKVURL}"
                sed -i 's|_STAFFLINEAPIURL_|${STAFFLINEAPIURL}|g' appsetting.json; \
                sed -i 's|_DATABASECONNECTIONSTRING_|${DATABASECONNECTIONSTRING}|g' appsetting.json; \
                sed -i 's|_ISSUER_|${ISSUER}|g' appsetting.json; \
                sed -i 's|_AKVURL_|${AKVURL}|g' appsetting.json;
                """
            }
        } 
        // stage("sed files"){
        //     environment{
        //         // StafflineApiUrl = 'http://stafflineapi-beta.compunnel.com/'
        //         STAFFLINEAPIURL = '"http://stafflineapi-beta.compunnel.com/"'
        //         DATABASECONNECTIONSTRING = '"Data Source=20.235.248.163;Initial Catalog=csg_2001_Staging; MultipleActiveResultSets=True; User ID=csgdbstaging; Password=djj8@s#sjs;Persist Security Info=True;Pooling=Yes;"'
        //         ISSUER = '"https://dev-api.velocityln.ai/"'
        //         AKVURL = '"https://vln-key-vault.vault.azure.net/"'
        //     }
        //     steps{
        //         sh """
        //         echo "${STAFFLINEAPIURL}"
        //         echo "${ISSUER}"
        //         echo "${AKVURL}"
        //         sed -i 's|_STAFFLINEAPIURL_|${STAFFLINEAPIURL}|g' appsetting.json; \
        //         sed -i 's|_DATABASECONNECTIONSTRING_|${DATABASECONNECTIONSTRING}|g' appsetting.json; \
        //         sed -i 's|_ISSUER_|${ISSUER}|g' appsetting.json; \
        //         sed -i 's|_AKVURL_|${AKVURL}|g' appsetting.json;
        //         """
        //     }
        // }    
    }
}

def getEnvName(branchName) {
    if("main".equals(branchName)) {
        return "uat";
    } else if ("pre-production".equals(branchName)) {
        return "qa";
    }else if ("development".equals(branchName)){
        return "dev";
    }
}
