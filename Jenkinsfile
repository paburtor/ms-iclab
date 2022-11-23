def responseStatus = ''
def myscript

pipeline {
    agent any
    
     parameters 
    {        
        choice(name: "Tool", choices: ["gradle", "maven"], description: "Seleccione la herramienta de construcción")
        booleanParam(name: "upload", defaultValue: true, description: "¿Actualizar repositorio Nexus?")
    }
        
    stages {
        stage('INFO')
        {
            steps{
                echo 'Info..............'                
                //sh 'printenv'
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
        
      
        stage('Build')
        {
            steps
            {
                script
                {                    
                    gitTag=sh(returnStdout: true, script: "git tag --contains | head -1").trim()                     
                    echo gitTag
                    /*                    
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
                    */
                }                                 
            }
            post {
                success {
                    echo 'Build Success ' + params.Tool
                    //slackSend color: "good", message: "Build Success"
                }
                failure {
                    echo 'Build Failed ' + params.Tool
                    //slackSend color: "danger", message: "Build Failed"
                }
            }
        }     
    }
}
