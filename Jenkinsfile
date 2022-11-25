def pipelineType

def mns(pipeline,branch,stagename,status){
    def mensaje = "Grupo 3 - " + pipeline + " - Rama : " + branch + " - Stage : " + stagename + " - " + status
    return mensaje
}

pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment{
        commitId = sh(returnStdout: true, script: 'git rev-parse HEAD')
        commitcheck = sh(returnStdout: true, script: '[ "$CHANGE_BRANCH" = "" ] || git rev-parse origin/$CHANGE_BRANCH')
        result = sh(returnStdout: true, script: 'git log --format=%B HEAD -n1')
        author = sh(returnStdout: true, script: 'git log --pretty=%an HEAD -n1')
        GIT_COMMIT_USERNAME = sh (script: 'git show -s --pretty=%an', returnStdout: true ).trim()
        commitmsg = sh(returnStdout: true, script: 'git log --oneline -n 1 HEAD')
        projectname = sh(script: 'git remote get-url origin | cut -d "/" -f5', returnStdout: true)
        commiteremail = sh(returnStdout: true, script: 'git log --pretty=%ae HEAD -n1')
        jenkinsurl = sh(script: 'echo "${BUILD_URL}"' , returnStdout: true).trim()
    }
    //[Grupo2][Pipeline IC/CD][Rama: develop][Stage: build][Resultado: Éxito/Success].
    //[Grupo2][Pipeline IC/CD][Rama: re-v1-0-0][Stage: test][Resultado: Error/Fail].
    stages {
        stage("Env Variables") {
            steps {
                sh "printenv"
                script {
                    if(env.BRANCH_NAME == 'main'){
                        pipelineType = "Pipeline CD"
                    }else{
                        pipelineType = "Pipeline CI"
                    }
                }
            }
        }
        stage("Build"){
            when { anyOf { branch 'feature-*'; branch 'main' } }
            steps {
                sh './mvnw clean compile -e'
                // slackSend color: "good", message: "Building.. branch: "+env.BRANCH_NAME
            }
            post{
                success{
                    def mensaje = mns(pipelineType,env.BRANCH_NAME,env.STAGE_NAME,'Success')
                    slackSend color: "good", message: mensaje
                }
                failure {
                    def mensaje = mns(pipelineType,env.BRANCH_NAME,env.STAGE_NAME,'Fail')
                    slackSend color: "danger", message: mensaje

                }
            }
        }
    }
}