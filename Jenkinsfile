pipeline{
    agent{
        label 'dotnet8'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM ('H * * * 1-5')
    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'master')
        choice(name: 'CONFIGURATION', choices: ['Debug','Release'])
    }
    tools{
        dotnet 'dotnetsdk'
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
                sh 'dotnet publish -c "${params.CONFIGURATION}" --output ./published ./src/Presentation/Nop.Web/Nop.Web.csproj'
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
        }
    }
}