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
```terraform workspace new <env>``` and ```terraform workspace select <env>``` ,
(You will need to create separated .tfvars files, for each environment.)

# Layers

Splitting huge code into smaller pieces is what we do when developing microservices. Then we can use the same approach for infrastructure; one state file per layer and per workspace,  this allows to each team member, make changes to different layers without conflicting with coworkers' modifications to other layers. 

Then we can define possible 3 types of layers:

Bootstrap: used to kickstart your project, set up the backend, the organization, the default roles,  lock down some security parts (enable Cloudtrail for example). Should be cloud provider independent. You can use local tf.state local and backup somewhere.

Foundation: provisions resources like VPC networks and subnets, security policies, even databases. One option is to store everything in a single repo, where each team can have its own state file.

Service: includes everything else regarding your business activity; Instances, clusters, containers, buckets for services, opening ports and load balancers to expose some things.

With two layers that are less frequently applied than the third one, so then they will not be affected by small changes to services, reducing as well as the execution time for each plan.

The service layer can be as well divided in smaller pieces, one of the best strategy  is to use an objected oriented approach (instead of environment oriented approach), but try to not make layer so small, so you would be end managing a lot of layers for a simple implementation, then for sizing a layer correctly, keep in mind these ideas:

The target size of the project

making the layer meaningful

minimizing the dependencies between layers

The number of concurrent collaborators (during the build phase and at runtime)

# Separation
State separation signals prevents from affecting different environments deployments affect each other. There are two primary methods to separate state between environments: directories and workspaces.

To separate environments with potential configuration differences, use a directory structure. Use workspaces for environments that do not greatly deviate from one another, to avoid duplicating your configurations.

# Tools

Use Terraform CLI in the Github Actions Runner as part of the workflow to deploy infrastructure:

hashicorp/setup-terraform: Sets up Terraform CLI in your GitHub Actions workflow.

Use terratest go library, to test deployment of resources

Use cookie-cutter similar to boiler plate the directory structure of new projects.
