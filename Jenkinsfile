// comment
pipeline {
 agent any
 stages {
        stage('Checkout-git ansible_pipeline_wso2apim'){
               steps{
		git poll: true, url: 'git@github.com:danibasconCHAKRAY/ansible_pipeline_wso2apim.git'
               }
        }
        stage('Checkout-git ansible_lanzamiento_vagrant'){
               steps{
		sh '''
			git clone git@github.com:danibasconCHAKRAY/ansible_lanzamiento_vagrant.git
               '''
		}
        }
            }
        }
  }
}
