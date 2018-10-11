// comment
pipeline {
 agent any
 stages {
        stage('Checkout-git ansible_pipeline_wso2apim'){
               steps{
		git poll: true, url: 'git@github.com:danibasconCHAKRAY/ansible_pipeline_wso2apim.git'
               }
        }
        stage('Running docker container'){
                steps{
                withCredentials([string(credentialsId: "c8ca2f47-777a-4ac1-85c8-c4b50c880f32", variable: "VMWARE")]) {
                        sh '''
                                set +x
                                cd ansible_lanzamiento_vagrant
                                #export esxi_password=\$VMWARE
                                vagrant ssh-config | grep -oE "([0-9]{1,3}[.]){3}[0-9]{1,3}" > ip.txt
                                echo "inspec exec test-wso2apim.rb -b ssh --host $(cat ip.txt) --user vagrant -i /root/.ssh/private_key --sudo" > script.sh
                                chmod +x script.sh
                                cd ../
                                docker run -dit --name test-`date +%y-%m-%d` -v $(pwd)/ansible_lanzamiento_vagrant/provisioning/roles/ansible-wso2apim/tests/:/devops/source \
                                -v $(pwd)/ansible_lanzamiento_vagrant/.vagrant/machines/`ls ansible_lanzamiento_vagrant/.vagrant/machines`/vmware_esxi/private_key:/root/.ssh/private_key  \
                                chakray/platform:1.0.0 bash
                                docker cp ansible_lanzamiento_vagrant/script.sh test-`date +%y-%m-%d`:/devops/source
                                docker cp ansible_lanzamiento_vagrant/ip.txt test-`date +%y-%m-%d`:/devops/source
                        '''
                        }
                }
        }
        stage('Running test'){
                steps{
                        sh '''
                                docker exec -it test-`date +%y-%m-%d` sh script.sh
                        '''
                }
        }        
        stage('Poweroff docker container'){
                steps{
                        sh '''
                                docker stop test-`date +%y-%m-%d`
                                docker rm test-`date +%y-%m-%d`
                        '''
                }		        

        }
        stage('Poweroff vagrant machine'){
                steps{
                        withCredentials([string(credentialsId: "c8ca2f47-777a-4ac1-85c8-c4b50c880f32", variable: "VMWARE")]) {
                        sh '''
                                set +x
                                export esxi_password=\$VMWARE
                                cd ansible_lanzamiento_vagrant/
                                vagrant destroy -f
                        '''
                        }		        
                }

        }
        stage('Pipeline finished'){
                steps{
                        sh '''
                                echo "Finished, Ok all."
                        '''
                }
        }        
  }
}
