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
                        mkdir ansible_lanzamiento_vagrant/provisioning/roles

               '''
		}
        }
        stage('Checkout-git ansible-wso2apim'){
               steps{
                sh '''
                        cd ansible_lanzamiento_vagrant/provisioning/roles/
                        git clone git@github.com:danibasconCHAKRAY/ansible-wso2apim.git
               '''
                }

        }
        stage('Checkout-git ansible-oracle-java'){
               steps{
                sh '''
                        cd ansible_lanzamiento_vagrant/provisioning/roles/
                        git clone https://github.com/falconmfm/ansible-oracle-java.git
               '''
                }

        }
  }
}
