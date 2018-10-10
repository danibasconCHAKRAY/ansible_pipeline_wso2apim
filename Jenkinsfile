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
                        if [ ! -d ansible_lanzamiento_vagrant ] ; then
			        git clone git@github.com:danibasconCHAKRAY/ansible_lanzamiento_vagrant.git
                        fi
                        if [ ! -d ansible_lanzamiento_vagrant/provisioning/roles ] ; then
                                mkdir ansible_lanzamiento_vagrant/provisioning/roles
                        fi

               '''
		}
        }
        stage('Checkout-git ansible-wso2apim'){
               steps{
                sh '''
                        if [ ! -d ansible_lanzamiento_vagrant/provisioning/roles/ansible-wso2apim ] ; then
                                cd ansible_lanzamiento_vagrant/provisioning/roles/
                                git clone git@github.com:danibasconCHAKRAY/ansible-wso2apim.git
                        fi
               '''
                }

        }
        stage('Checkout-git ansible-oracle-java'){
               steps{
                sh '''
                        if [ ! -d ansible_lanzamiento_vagrant/provisioning/roles/ansible-oracle-java ] ; then
                                cd ansible_lanzamiento_vagrant/provisioning/roles/
                                git clone https://github.com/falconmfm/ansible-oracle-java.git
                        fi
               '''
                }

        }
        stage('Defining the variables for ansible-oracle-java'){
               steps{
                sh '''
                        if [ $(cat ansible_lanzamiento_vagrant/provisioning/group_vars/all.yml | wc -l) == 1 ] ; then
                                echo -e " java_version: 8 \n java_subversion: 141 \n java_build: 15 \n jdk_tarball_hash: 336fa29ff2bb4ef291e347e091f7f4a7" > ansible_lanzamiento_vagrant/provisioning/group_vars/all.yml
                        fi
               '''
                }

        }
        stage('Defining the playbook for ansible'){
               steps{
                sh '''
                        if [ $(cat ansible_lanzamiento_vagrant/provisioning/playbook.yml | wc -l) == 6 ] ; then
                                echo "      - ansible-wso2apim" >> ansible_lanzamiento_vagrant/provisioning/playbook.yml
                        fi
               '''
                }

        }
        stage('Running Vagrant machine'){
                steps{
                withCredentials([string(credentialsId: "c8ca2f47-777a-4ac1-85c8-c4b50c880f32", variable: "VMWARE")]) {
                        sh '''
                                set +x
                                cd ansible_lanzamiento_vagrant
                                #vagrant plugin install vagrant-vmware-esxi
                                export esxi_password=\$VMWARE
                                vagrant up --provider=vmware_esxi --provision 
                        '''
                        }		        
                }

        }
        stage('Running docker container'){
                steps{
                        sh '''
                                echo "inspec exec test-wso2apim.rb -b ssh --host ip --user vagrant -i /root/.ssh/private_key --sudo"
                                docker run -it --rm -v  ansible_lanzamiento_vagrant/provisioning/roles/ansible-wso2apim/tests/:/devops/source -v \
                                ansible_lanzamiento_vagrant/.vagrant/machines/`ls ansible_lanzamiento_vagrant/.vagrant/machines`/vmware_esxi/private_key:/root/.ssh/private_key  \ 
                                 chakray/platform:1.0.0 bash
                        '''
                }
        }
  }
}
