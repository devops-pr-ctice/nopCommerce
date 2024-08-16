pipeline{
    agent{ label 'DotNet' }
    options{
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers{
        pollSCM('H * * * *')
    }
    stages{
        stage('checkout'){
            steps{
                    git url: 'https://github.com/devops-pr-ctice/nopCommerce.git',
                        branch: 'develop'
            }
        }
        stage('build'){
            steps{
                sh 'mkdir -p ./published'
                sh 'dotnet build --configuration Release --output ./published ./src/Presentation/Nop.Web/Nop.Web.csproj'
            }
        }
    }
    post{
        success{
            zip zipFile: 'published/Nop.Web.zip',
                archive: 'true',
                dir: './published'
            archiveArtifacts artifacts: 'published/Nop.Web.zip'
        
        }
    }
}