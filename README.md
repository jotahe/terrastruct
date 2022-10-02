 # TerraStruct

 # best practices to keep your project stable, readable and scalable:

- Keep changes small to reduce blast radius: because many changes imply risks of breaks.
- Review your code: even if your code works, ask for a review to share the changes and challenge its implementation.
- When possible, promote to production: apply your infrastructure changes to a homologation environment to test them and   
  then apply to other environments (quick reminder: staging and testing environments are production environments for OPS).
- As with every code, naming conventions are essential: You need to define them beforehand to avoid resource recreation.
- Wait before automating: as there will be a high-frequency building phase and a runtime phase.

# Monolith
This approach consist on holding all the configuration in a ```single state file```
It exists a separation between files for readability, and there are modules to wrap reused code, however one online terraform state file.

- Terraform handles intra-project dependencies: since you apply all your code at once, you can make references to objects from one file to another. An example of this is managing dependencies to the VPC. This is the network resource where you'll be binding other infrastructure blocs like the Kubernetes cluster or the databases. In your postgresql.tf and kubernetes.tf, you can refer to the VPC created by vpc.tf using aws_vpc.main.id
- Terraform plans are executed over the entire infrastructure: this means that every time you commit a new change, you will be checking that the whole infrastructure matches the code you applied. This method enforces correctness on the state of the infrastructure, which is a good thing.
- You can use workspaces to replicate through isolated environments; as Terraform will create one state file per each. So you'll have testing, staging, integration, production workspaces, and so on, by just using the commands:
```terraform workspace new <env>``` and ```terraform workspace select <env>``` 
You will need to create separated .tfvars files, for each environment.