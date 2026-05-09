pipeline{
    agent {
        label "node1"
    }
    tools{
        maven "maven123"
    }
    options{
        skipDefaultCheckout(true)
    }
    stages{
        stage("clone"){
            steps{
            checkout scm
            }
        }
        stage("build"){
            steps{
                echo "build stage2"
                sh"mvn clean package"
            }
        }
        stage("deploy"){
            steps{
                echo "hello"
                sh"""
                    cp /home/ubuntu/jenkins/workspace/demo/target/*.war /opt/tomcat/webapps/
                """
                dir("/opt/tomcat/webapps/"){
                    sh"""
                    jar -xvf *.war
                    cp -r /opt/tomcat/webapps/java-tomcat-maven-example/* ROOT/

                    """
                }
            }
            
        }
        stage("cleanup"){
            steps{
                sh"rm -rf /opt/tomcat/webapps/java-tomcat-maven-example*"
            }
        }
    }
}