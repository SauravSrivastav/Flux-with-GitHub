Step1: Create a GitHub Repository Under Your Own Account

	Ø To create a repository on GitHub you must log in to your own account, and then you can create a repository with the YAML files you require. Or you can find the linuxacademy/content-gitops repository and fork it. Once you create your own version of that repository, examine the YAML files in the namespaces and workloads folders.

	git config --global user.email "sauravsrivastav2205@gmail.com"
	git config --global user.name "Saurav Srivastav"

Step2: Deploy Flux Into Your Cluster

	Ø To check whether fluxctl is installed, enter:
	fluxctl version

	Ø If fluxctl did not install automatically, you may enter the following command to install it:
	For linux: sudo snap install fluxctl --classic
	For Mac: brew install fluxctl
	
	Ø Create a namespace for Flux:
	kubectl create ns flux
	
	Ø Set the GHUSER environment variable:
	export GHUSER=[Your GitHub Handle]
	export GHUSER=SauravSrivastav
	env | grep GH
	
	Ø Deploy Flux using the fluxctl command:
     fluxctl install --git-user=${GHUSER} --git-email=${GHUSER}@users.noreply.github.com --git-url=git@github.com:${GHUSER}/Flux-with-GitHub --git-path=namespaces,k8 --git-branch=main --namespace=flux | kubectl apply -f -
	
Step3: Verify The Deployment and Obtain the RSA Key

	Ø Verify the Flux deployment:
	kubectl get pods --all-namespaces
	kubectl -n flux rollout status deployment/flux
	
	Ø Obtain the Flux RSA key created by fluxctl:
	fluxctl identity --k8s-fwd-ns flux
	
Note: Copy off the RSA key to implement in GitHub.
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDu6QjGaEL5/fEtPCYCIm3pQxAtJ+qdceTwpNObI8hW49AqJTwaU0mFetoRBsVvpWioazUz/QCEzc0tk01+n6G7Vg1LGCVWeWt9wjLkyJp99KIED9N2eUi83k5GQhgY6qfshRoGIdPSJ6l3WoWgYCfUUElHTS+D7cvK6JDYTAy/6mQMtwDoygHqc5WdLJONx2hJ2OTAKlZ7+wZj0q2PB5HGfGRQSCHAcZbEdKW5mppzDtThBynWIXPza+HBHTSPObmO0tK5BRcCwjQx+FAAmM7bxXsK1894Suxe/C6lpR8L4c3yM0wYUU4AJ1GDa5dcdr9Qk+5d4hOPT08KOZcgUTve3pgYAra4rJV99O6OyAg6rQB+iPx3xWShB8ZQy59OcluhWk5bTVipde0MjsGr+GwBx1T9ApJ/mrL8Ax8KnVqxja9xGsMk73jITrcjAZK+B7763sdYJQrk/47UwJcPsLF3xen0YBTXsvKNyNMDOM26EfLZZA+U7mY7UjaVIrjFmqk= root@flux-7bd9d4c66-flggh

Step4: Implement the RSA Key in GitHub
Use the GitHub User Interface to Add the RSA Key obtained as a Deploy Key in GitHub.
In the GitHub user interface, make sure we're in our new repository and click on the Settings tab. In there, click Deploy keys, then click the Add deploy key button. We can give it a Title of something like GitOps Deploy Key, then paste the key we copied earlier down in the Key field. Check the Allow write access box, and then click Add key.

Step5: Use the fluxctl sync Command to Synchronize the Cluster with the Repository

	Ø Use fluxctl to sync the cluster with the repository:
	fluxctl sync --k8s-fwd-ns flux
	
	Ø Then check the existence of the lasample namespace:
	kubectl get namespaces
	
	Ø Finally check that the Nginx deployment is running:
	kubectl get pods --all-namespaces
	
	
