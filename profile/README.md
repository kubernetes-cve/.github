## Challenge

Design & Develop a chatbot that can answer questions about CVEs and their associated threats. The chatbot should be able to answer questions like:

Which CVE affected GlobalProtect App by Palo Alto Networks? Describe in detail.
What is the CVE for the vulnerability in the Apache HTTP Server 2.4.46 and how can I mitigate it?
All of this resources need to be designed for AWS EKS

All resources should be highly available and scalable to handle a large number of requests

The Cluster should be secured using a service mesh and must use mutual TLS for service to service communication

Logging and Monitoring should be implemented for all the services

The entire cluster along with the addons must be bootstrapped using Terraform

![cluster-arch](https://github.com/user-attachments/assets/a8e827c4-fc09-4d64-86ac-3126f06bd719)


## Solution

CVE Threat Lens is a combination of 4 Microservices that work together to provide a chatbot that can answer questions about CVEs and their associated threats. The Chatbot uses these vector embeddings to answer questions about CVEs and their associated threats.

Producer Microservice: The first microservice written using golang is the Producer which is responsible for sending CVE data to the Kafka topic.

Consumer Microservice The second microservice is the Consumer which consumes the data from the Kafka topic and stores it in the Postgres Database.

Kubernetes Operator Kubernetes Operator that fetches the latest CVE Records and then creates a processor job every hour.

Embeddings Job A job that fetches the records present in the postgres database and then creates vector embeddings for each record while storing them in PGVECTOR.

Chatbot The chatbot is built using Streamlit and uses the embeddings to answer questions about CVEs and their associated threats.

Cluster Security Features The cluster is secured using a istio service mesh for service to service communication and a network policy that only allows traffic from the istio sidecar. Cert-manager is used to manage the certificates for the services.

Monitoring and Metrics Prometheus is used to store metrics from all the services and Grafana is used to visualize them. Custom metric scrapers are written for kafka, postgres and the chatbot.

CI/CD Packer is used to create a custom Jenkins AMI that is used to run the CI/CD pipeline. The pipeline is written using Groovy scripts and everything is baked into the AMI. Semantic versioning is used for the docker images and helm charts.

<img width="4825" height="2755" alt="chatbot_arch" src="https://github.com/user-attachments/assets/0f65faec-e526-4cf0-b471-f0f87e22cf44" />

