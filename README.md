# Deployment of Jmeter cluster in AWS EKS

## Deployment Topology
![k8](https://user-images.githubusercontent.com/47892366/135235283-5b14aa1c-12c2-4f0c-8d64-a9b7a527d1ae.png)

The test can be started from the master node, and the master node sends the corresponding test script to the corresponding slaves node. 
The pod/nodes of the slave nodes is mainly used to generate pressure.

## Files with Description
1. Dockerfile base - Build Jmeter basic image

2. Dockerfile master - Build Jmeter master image

3. Dockerfile slave - Building Jmeter slave image


4. jmeter_cluster_create.sh - this script will require a unique namespace, and then it will continue to create the namespace and all components 
(jmeter master, slaves, infuxdb, and grafana).

Note: before starting, please click jmeter_slaves_deploy.yaml Set the number of copies to be used for the slaves server in the file. 
Usually, the number of copies should match the worker nodes that you have.

5. jmeter_master_configmap.yaml - Application configuration of Jmeter master.

6. jmeter_master_deploy.yaml - Deployment list of Jmeter master.

7. jmeter_slaves_deploy.yaml - Deployment list of Jmeter slave.

8. jmeter_slave_svc.yaml - service list of jmeter slave. Using headless service enables us to directly obtain the POD IP address of jmeter slave. 
This file is created to make it easier for slave Pod IP addresses to be sent directly to jmeter master.

9. jmeter_influxdb_configmap.yaml - application configuration of influxdb deployment.
10. jmeter_influxdb_deploy.yaml - deployment manifest of Influxdb

11. jmeter_influxdb_svc.yaml - list of services for Influxdb.
12. jmeter_grafana_deploy.yaml - grafana deployment checklist.
13. jmeter_grafana_svc.yaml - list of services deployed by grafana. NodePort is used by default. If it is running in the public cloud, it can be changed to LoadBalancer.
14. dashboard.sh - this script is used to automatically create the following:
      
      (1) An influxdb database (Jmeter) in influxdb pod
  
      (2) Data source in grafana (jmeterdb)
