pipeline{
	agent any
	environment{
		EC2_user = 'ubuntu'
		EC2_HOST = '13.204.62.64'
		SSH_CREDENTIAL_ID = 'ec2-ssh-key'
		}
		stages{
		stage("Build"){
				steps{
					echo "Start Building"
					sh "npm install"
					sh "npm run build"
					echo "Building Completed"
				}
			}
		stage("Deploy"){
				steps{
					echo "Staring Deployment on EC2....."
					sshagent(['ec2-ssh-key']){
						sh '''
							echo "Copying build artifacts to EC2"
							scp -o StrictHostKeyChecking=No -r dist/ ubuntu@13.204.62.64:/var/www/html
							echo "Restarting web server on EC2"
							scp -o StrictHostKeyChecking=No ubuntu@13.204.62.64 'sudo system restart nginx'
							'''
							}
							echo 'Deployement Completed'
							}
			}

	}
}