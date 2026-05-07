pipeline{

agent none

    options {
    skipDefaultCheckout true
    }

   stages{
 
    stage("build"){
        agent{
            label 'node2'
        }
        steps{
            echo "build stage"
            checkout scm
            sh "mvn package"
            stash name: 'build', includes: 'target/*.war'
        }
    }
    stage('unstash'){
        agent{
            label 'node1'
        }
        steps{
            sh "mkdir -p unstash"
            dir('unstash'){
                unstash 'build'
            }
        }
    }
    stage('deploy'){
        agent{
            label 'node1'
        }
        steps{
         sh "sudo rm -rf /opt/tomcat/myapp_dir/*"
        dir('unstash/target/'){
            sh "sudo mv *.war /opt/tomcat/myapp_dir/ && cd /opt/tomcat/myapp_dir/ &&  \
            sudo jar -xvf *.war && \
            sudo rm -f *.war "
        }
        }

    }
    stage('restart tomcat'){
        sh "sudo systemctl restart tomcat.service"
    }
    
   }
}
