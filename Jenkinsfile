pipeline{
    agent{
        label 'dev-node'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM ('H/30 * * * *')
    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'master')
        choice(name: 'CONFIGURATION', choices: ['Debug','Release'])
    }
    tools{
        dotnetsdk 'dotnetsdk'
    }
    stages{
        stage('SCM'){
            steps{
                git url: 'https://github.com/devops-pr-ctice/nopCommerce.git',
                branch: "${params.BRANCH}"
            }
        }
        stage(build){
            steps{
                sh 'mkdir -p ./published'
                sh  "dotnet publish -c ${params.CONFIGURATION} --output ./published ./src/Presentation/Nop.Web/Nop.Web.csproj"
            }
        }
    }
    post{
        success{
            zip zipFile: 'published/Nop.Web.zip',
                archive: 'true',
                dir: 'published',
                overwrite: 'true'
            archiveArtifacts artifacts: 'published/Nop.Web.zip'

            mail to: 'pramod.pramod36@gmail.com',
                 subject: "Build suceed: ${env.BUILD_ID}, ${env.BUILD_DISPLAY_NAME}",
                 body:"The build was successful. Check it out at: ${env.BUILD_URL}"
        }
        failure{
            mail to: 'pramod.pramod36@gmail.com',
                 subject: "Build failure: ${env.BUILD_ID}, ${env.BUILD_DISPLAY_NAME}",
                 body:"The build was failure. Check it out at: ${env.BUILD_URL}"
        }
    }
}