pipeline{
    agent any
    
    
    parameters {
        string defaultValue: 'master', description: 'Provide Branch Name', name: 'BRANCH'
    }

    stages{
        stage("Checkout"){
            steps{
                echo "========executing Checkout========"
                script{
                    sh(script: "git checkout ${BRANCH}")
                }
            }
            
        }
        stage("Deploy"){
            steps{
                echo "========executing Deploy========"
                script{
                    
                    sh(script:"ssh -o StrictHostKeyChecking=no   ubuntu@35.173.211.162 \"mkdir -p /home/ubuntu/prometheus-grafena-2/ \" ")
                    sh(script:"scp -r ${WORKSPACE}/* ubuntu@35.173.211.162:/home/ubuntu/prometheus-grafena-2/ ")
                    sh(script:"ssh -o StrictHostKeyChecking=no  ubuntu@35.173.211.162 \"cd /home/ubuntu/prometheus-grafena-2/ && sudo docker-compose up -d \" ")
                }
                
            }
            
        }
        stage("Status"){
            steps{
                echo "========executing Status========"
                script{
                    sh(script: "ssh -o StrictHostKeyChecking=no   ubuntu@35.173.211.162 \n")
                    sh '''#!/bin/bash
                       RUN1=`docker-compose ps -q prometheus`
                       RUN2=`docker-compose ps -q grafana`
                       if [[ "$RUN1" != "" ]] &&  [[ "$RUN2" != "" ]]
                       then
                       echo "The service is running!!!"
                       else
                       echo "====++failure++++===="
                       fi'''
                }
            }
            
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}

