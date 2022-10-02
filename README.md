 # TerraStruct

 # best practices to keep your project stable, readable and scalable:

- Keep changes small to reduce blast radius: because many changes imply risks of breaks.
- Review your code: even if your code works, ask for a review to share the changes and challenge its implementation.
- When possible, promote to production: apply your infrastructure changes to a homologation environment to test them and   
  then apply to other environments (quick reminder: staging and testing environments are production environments for OPS).
- As with every code, naming conventions are essential: You need to define them beforehand to avoid resource recreation.
- Wait before automating: as there will be a high-frequency building phase and a runtime phase.

# Monolith
This approach consist on holding ´´´all´´´ the configuration in a ```single state file```