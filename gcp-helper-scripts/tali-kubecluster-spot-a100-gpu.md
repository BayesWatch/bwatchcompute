## Starting a GPU cluster
Run command:

```bash
gcloud beta container --project "tali-multi-modal" clusters create "spot-gpu-cluster-1" --zone "us-central1-a" \
--no-enable-basic-auth --cluster-version "1.23.12-gke.100" --release-channel "regular" --machine-type "a2-highgpu-1g" \
--accelerator "type=nvidia-tesla-a100,count=1" --image-type "UBUNTU_CONTAINERD" --disk-type "pd-standard" --disk-size "100" \
--metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/cloud-platform" --max-pods-per-node "110" \
--spot --num-nodes "1" --logging=SYSTEM,WORKLOAD --monitoring=SYSTEM --enable-ip-alias \
--network "projects/tali-multi-modal/global/networks/default" \
--subnetwork "projects/tali-multi-modal/regions/us-central1/subnetworks/default" --no-enable-intra-node-visibility \
--default-max-pods-per-node "110" --enable-autoscaling --min-nodes "0" --max-nodes "16" --no-enable-master-authorized-networks \
--addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver,GcpFilestoreCsiDriver --enable-autoupgrade \
--enable-autorepair --max-surge-upgrade 10 --max-unavailable-upgrade 0 --autoscaling-profile optimize-utilization \
--resource-usage-bigquery-dataset "tali_billing_us" --enable-network-egress-metering --enable-resource-consumption-metering \
--workload-pool "tali-multi-modal.svc.id.goog" --enable-shielded-nodes --node-locations "us-central1-a"


Once it's done, install the gpu management driver by running:

```bash
kubectl apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/container-engine-accelerators/master/nvidia-driver-installer/ubuntu/daemonset-preloaded.yaml
```