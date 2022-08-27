#!groovy

@Library("workflowlibs") _

pipeline {
    agent {
        kubernetes {
          label 'grapi-agent'
          yamlFile 'agent.yaml'
        }
      }

  parameters {
    choice(choices: '\nMAJOR\nMINOR\nPATCH', description: 'Semantic version for new version of the API RAML. MAJOR version when you make incompatible API changes, MINOR version when you add functionality in a backwards-compatible manner and PATCH version when you make backwards-compatible bug fixes.', name: 'version')
    choice(choices: 'ACTIVE\nPLANNED', description: 'Publish of the new version will generate an active version that can be considered as final (ACTIVE) or a draft version for review (PLANNED).', name: 'definition')
    choice(choices: '\nGLOBAL_GLOBAL\nGLOBAL_MULTILOCAL\nGLOBAL_LOCAL\nLOCAL_LOCAL', description: 'Relationship between the type of geography of the definition of an API and its implementation.', name: 'type')
  }

  stages {
    stage('Executing validation repository') {
      steps {
        script {
          globalBootstrap {
            libraryName     = "bpc-workflowlibs"
            libraryBranch   = "master"

            entrypointParams = [
              nodeLabel     : "global",
              type          : "API",
              version       : "v1"
            ]
          }
        }
      }
    }
  }

  post {
    success {
      echo 'Pipeline finished successfully!'
    }
    unstable {
      echo 'I am unstable :/'
    }
    failure {
      echo 'I failed :('
    }
    changed {
      echo 'Things were different before...'
    }
    aborted {
      echo "Aborted"
    }
  }
}
