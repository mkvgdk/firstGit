pipeline {
  agent any
  stages {
    stage('') {
      agent {
        docker {
          image 'node:10-alpine'
          args '-p 20001-20100:3000'
        }

      }
      environment {
        CI = 'true'
        HOME = '.'
        npm_config_cache = 'npm-cache'
      }
      steps {
        sh '''pipeline {
  agent {
    docker {
      image \'node:10-alpine\'
      args \'-p 20001-20100:3000\'
    }
  }
  environment {
    CI = \'true\'
    HOME = \'.\'
    npm_config_cache = \'npm-cache\'
  }
  stages {
    stage(\'Install Packages\') {
      steps {
        sh \'npm install\'
      }
    }
    stage(\'Test and Build\') {
      parallel {
        stage(\'Run Tests\') {
          steps {
            sh \'npm run test\'
          }
        }
        stage(\'Create Build Artifacts\') {
          steps {
            sh \'npm run build\'
          }
        }
      }
    }
    stage(\'Deployment\') {
      
        stage(\'Staging\') {
          when {
            branch \'staging\'
          }
          steps {
            withAWS(region:\'us-west-2a\',credentials:\'mkvgdk@outlook.com\') {
              s3Delete(bucket: \'firstGIT\', path:\'**/*\')
              s3Upload(bucket: \'firstGIT\', workingDir:\'build\', includePathPattern:\'**/*\');
            }
            mail(subject: \'Staging Build\', body: \'New Deployment to Staging\', to: \'jenkins-mailing-list@mail.com\')
          }
        }
        
        
      
    }
  }
}'''
        }
      }
    }
  }