pipeline{
			agent {
					node {
							label 'built-in'
							customWorkspace '/root/demo/'
					}
				}

			stages{
					stage('git-clone-to-master') {
							steps{
									sh "rm -rf *"
									sh " yum install git -y "
									sh "git clone https://github.com/ashrayp18/sample-hello.git"
							}
					
					}
					stage('Build') {
							steps{
							dir('/root/demo/sample-hello'){
									sh " mvn clean package"	
									sh "scp  -i /root/test /root/demo/sample-hello/target/my-webapp-1.1.war ec2-user@172.31.10.102:/home/ec2-user/demo/"
									sh " cp /root/demo/sample-hello/target/my-webapp-1.1.war /root/tools/tomcat/apache-tomcat-9.0.82/webapps"
									}
								}	
					
					}
					
					
					stage('tomcat-install-on-slave1'){
								agent {
										node {
											label 'qa'
											customWorkspace '/home/ec2-user/test/'
											}
										}	
						steps{	
								sh " sudo rm -rf * "
								sh " sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.82/bin/apache-tomcat-9.0.82.zip"
								sh " sudo unzip apache-tomcat-9.0.82.zip"
								sh " sudo rm -rf apache-tomcat-9.0.82.zip"
								sh " sudo chmod -R 777 apache-tomcat-9.0.82"
							}
					
						}
						stage('deploy-on-slave1'){
								agent {
										node {
											label 'qa'
											}
										}	
						steps{	
								sh " sudo cp /home/ec2-user/demo/my-webapp-1.1.war /home/ec2-user/test/apache-tomcat-9.0.82/webapps/"
								sh " sudo chown -R ec2-user:ec2-user /home/ec2-user/test/apache-tomcat-9.0.82"
								sh " sudo /home/ec2-user/test/apache-tomcat-9.0.82/bin/./startup.sh"
								
								}
								
							}
					
						}
			}

