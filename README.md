#Below is the description of each component in the repo:

jmeter_cluster_create.sh — This script will ask for a unique tenant name (namespace) and then it will go ahead to create the namespace and all the components (jmeter master, slaves, influxdb and grafana).
N.B — Set the number of replicas you want to use for the slaves in the jmeter_slaves_deploy.yaml file before starting, normally the replicas should match the number of worker nodes that you have.
jmeter_master_configmap.yaml — The config map for the Jmeter master deployment
jmeter_master_deployment.yaml — The deployment manifest for the jmeter master.
jmeter_slaves_deploy.yaml — The deployment manifest for the jmeter slaves.
jmeter_slave_svc.yaml — The service manifest for the jmeter slave. It uses an headless service, this enables us to get just the jmeter slaves POD ip address directly, we don’t need the DNS or round robin for this. We created this so as to make easier to feed the slave pod IP addresses directly to the jmeter master, the advantage of this will be shown later.
jmeter_influxdb_configmap.yaml — The config map for the influxdb deployment. This configures the influxdb to exposes port 2003 so as to support the graphite if you want to use the graphite storage method in addition to the default influxdb port. So you can use the influxdb deployment to support both the jmeter backend listener methods (graphite and influxdb).
jmeter_influxdb_deploy.yaml — The deployment manifest for Influxdb
jmeter_influxdb_svc.yaml — The service manifest for the Influxdb.
jmeter_grafana_deploy.yaml — The grafana deployment manifest.
jmeter_grafana_svc.yaml — The service manifest for the grafana deployment, it uses NodePort by default, you can change this to LoadBalancer if you are running this in a public cloud (and maybe setup a CNAME to shorten the name with a FQDN).
jmeter_grafana_reporter.yaml — The deployment and service manifest of the reporter module.
dashboard.sh — This script is used to create the following automatically: (1) An influxdb database (jmeter) in the influxdb pod. (2) A datasource (jmeterdb) in the grafana pod
start_test.sh — This script is used to run the Jmeter test script automatically without you manually logging into the Jmeter master shell, it will ask for the location of the jmeter test script, then it will copy it to the jmeter master pod and initiate the test automatically towards the jmeter slaves.
GrafanaJMeterTemplate.json — A prebuilt jmeter grafana dashboard, this can also be found in the jmeter installation folder (extras folder).
