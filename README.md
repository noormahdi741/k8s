# neo4j-kubernetes + tack deployment

This project assumes that you have access to an AWS account and the appropriate administrative privileges to deploy resources.

Required Packages:
* awscli
* cfssl
* jq
* kubernetes-cli
* terraform

1. Create a project directory for  this specific deployment and enter it with the following commands
  * `mkdir neokube`
  * `cd neokube`
2. Clone the following repositories:
  * `git clone git@github.com:kz8s/tack.git`
  * `git clone git@github.com:mneedham/neo4j-kubernetes.git`
3. Enter the tack directory:
  * `cd tack`
4. Perform the following command to spin up a Kubernetes deployment on AWS:
  * `make all`
5. Enter the neo4j-kubernetes directory
  * `cd ../neo4j-kubernetes`
6. Create Core Neo4j containers on Kubernetes along with their service and StatefulSet:
  * `./neo4j.sh`
  * `kubectl create -f neo4j_svc.yaml`
  * `kubectl create -f neo4j_core.yaml`
7. Forward ports needed to access GUI and Bolt
  * `kubectl port-forward neo4j-core-0 7474:7474 7687:7687`
  * You can now access the GUI via `localhost:7474` and Bolt via `localhost:7687`
8. Scale the cluster up or down with the following commands (assuming you have 3 active pods):
  * Up: `kubectl scale statefulsets neo4j-core --replicas=4`
  * Down: `kubectl scale statefulsets neo4j-core --replicas=2`
9. Tear down environment
  * `cd ../tack && make clean`
  * Answer `yes` to the Terraform prompt
