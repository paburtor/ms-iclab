def responseStatus = ''
def myscript
def tagCommit
def comment

pipeline {
    agent any
    options { skipDefaultCheckout() } 
            
    stages {
       
        stage('Checkout SCM')
        {
            steps{
                
            checkout([
                $class: 'GitSCM',
                branches: scm.branches,
                doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
                extensions: [[$class: 'CloneOption', noTags: false, shallow: false, depth: 0, reference: '']],
                userRemoteConfigs: scm.userRemoteConfigs,
             ])
            }
        }
        
        stage('Check Commit comments')
        {
            steps
            {                                
                script
                {
                    //tagCommit=sh(script: 'git describe $GIT_COMMIT --abbrev=0', returnStdout: true)                    
                    commit=sh(script: 'git log -1 --format=format:"%H"', returnStdout: true)
                    comment=sh(script: 'git log --format=%B -n 1 $commit', returnStdout: true)
                    
                    echo "Commit :  $commit"
                    echo "Comment :  $comment"                    
                    
                    //echo tagCommit
                    //echo "El comentario del commit fue: $comment"
                    //echo 'El tag de commit fue : $tagCommit'
                                                           
                    if(comment.startsWith('Patch:') || comment.startsWith('Patch :')) 
                    {                        
                        echo "Commit Patch -> ($comment)"
                    }
                    if(comment.startsWith('Minor:') || comment.startsWith('Minor :')) 
                    {
                        echo "Commit Minor -> ($comment)"                        
                    }
                    else if (comment.startsWith("Major"))
                    {
                        echo "Commit Major -> ($comment)"                        
                    }
                    else
                    {
                        echo 'Error Git Comment....'                        
                        //throw new Exception("Error Git Comment ($tagCommit)")                                      
                    }    
                }                                                                  
            }
        }
/*                           
        stage('INFO')
        {
            steps{
                echo 'Info..............'                
                //sh 'printenv'
                //sh(script: 'git describe', returnStdout: true)
                echo sh(script: 'env|sort', returnStdout: true)

                //echo "Nombre tag: $TAG_NAME"
                //cleanWs()
                //slackSend color: "warning", message: "INFO: Prueba Taller 3 - Modulo 4 Branch: ${GIT_BRANCH}"
            }
            post {
                success {
                    echo 'INFO Success'
                    //slackSend color: "good", message: "Info Success. commit ${GIT_COMMIT}"
                }
                failure {
                    echo 'INFO Failed'
                    //slackSend color: "danger", message: "Info Failed."
                }
            }
        }
*/        
/*      
        stage('Build')
        {
            steps
            {
                script
                {              
                    //tagBuild=sh(script: 'git describe --tags --abbrev=0', returnStdout: true)
                    //tagBuild=sh(script: 'git describe $GIT_COMMIT --abbrev=0', returnStdout: true)                                        
                    //echo "El tag del Build es: ${tagBuild}"
                    //commentBuild=sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true)
                    //echo "El comment del Build es: ${commentBuild}"
                    
                    //currentBranch=sh(script: 'git branch --show-current', returnStdout: true)
                    //echo "Current branch: ${currentBranch}"                                       
                    
                    //sh(script: 'git switch main', returnStdout: true)
                    //commentBuild=sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true)                    
                    //sh(script: 'git switch $currentBranch', returnStdout: true)
                    //echo "El comment del main es: ${commentBuild}"
                                                                               
                    //echo "El comentario fue : ${GIT_COMMIT_MSG}"

                    //gitTag=sh(returnStdout: true, script: "git tag --contains | head -1").trim()                     
                    //echo gitTag
                                                                               
                    if(params.Tool == 'gradle') 
                    {
                        echo 'Invocando script gradle.groovy'
                        myscript = load 'gradle.groovy'                                            
                        myscript.call()   
                    }
                    else
                    {
                        echo 'Invocando script maven.groovy'
                        myscript = load 'maven.groovy'                        
                        myscript.call()   
                    }                    
                }                                 
            }
            post {
                success {
                    echo 'Build Success '
                    //slackSend color: "good", message: "Build Success"
                }
                failure {
                    echo 'Build Failed '
                    //slackSend color: "danger", message: "Build Failed"
                }
            }
        }
*/
    }
}
