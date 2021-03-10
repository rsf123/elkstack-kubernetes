# elkstack-kubernetes
Kubernetes yaml files to deploy elastiflow version 5 and elk

I use these deployment files to deploy ELK and Elastiflow on a bare metal kubernetes kluster which means there's a few prerequisites..

**Prerequisites** 
- Need to have a load balancer installed in order to expose Elastiflow for inbound Netflow traffic.  I use MetalLB 
- These deployment files are set up assuming there's an ingress proxy such as the NGINX Ingress Proxy
- The namespace is labelled to enable synchronization of a wildcard TLS certificate for use by the ingress service
- NFS storage configured for persistent volumes.  These deployment files are looking for a storage class named "nfs-storage".  I use the Kubernetes NFS Subdir External Provisioner

**To do**

Once the prerequisites are met, these deployment files can be used as is.  I do not have Elastic's security features turned on since this stack is running inside a private network.  Should you want to enable the security x-pack, the following steps need to be taken:

- In the elastic-config.yaml, uncomment the xpack.security.enabled line and the discovery.type line
- Configure Elastic's security features as described in the Elastic documentation
- base64 encode the Kibana and Elastiflow username and password that are being used by those services to access elasticsearch and add the credentials to the elastic-creds.yaml file
- If using anything other than Elastiflow Community edition, edit elastiflow-key.yaml to include base64 encoded strings of your account ID and license key
- In the kibana-config.yaml, uncomment the elasticsearch.username and elasticsearch.password lines
- In the kibana-ingress.yaml, add in your hostname and SSL certificates where indicated
- As an alternate, if you don't want to use the ingress proxy, deploy the kibana-svc-lb.yaml file instead and that will configure MetalLB to provide an address within your Load Balancer pool
