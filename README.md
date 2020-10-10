# gcs_gce_challenge_lab
GCP Challenge Lab with Google Cloud Storage and Google Compute Engine

Challenge:
- Create new project
- Create GCE instance that runs provided start-up script
- Create GCS bucket for log files
- Log files appear in GCS bucket after instance finishes starting up


Solution using CLI:
- Create project:
gcloud projects create gsc-gce-challengelab-cli2

- Link project to the billing account 
gcloud beta billing projects link gsc-gce-challengelab-cli2 --billing-account=XXXXXX-XXXXXX-XXXXXX

- Switch to the project 
gcloud config set project gsc-gce-challengelab-cli2

- Create GCS bucket
gsutil mb gs://gsc-gce-challengelab-iar-cli2

- Enable Compute Apis for creation of VM (GCS and Stackdriver are enabled by default, GCE is not enabled by default)
gcloud services enable compute.googleapis.com

- Create VM
gcloud beta compute --project=gsc-gce-challengelab-cli2 instances create gcs-and-gce-lab-cli-vm --zone=us-central1-a --machine-type=f1-micro --subnet=default --network-tier=PREMIUM --metadata=^,@^lab-logs-bucket=gs://gsc-gce-challengelab-iar-cli2 --metadata-from-file startup-script='GCS&GCE_Challenge_Lab/start-up_script.sh' --maintenance-policy=MIGRATE --scopes=https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/trace.append,https://www.googleapis.com/auth/devstorage.read_write --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=gcs-and-gce-lab-cli-vm --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=purpose=challengelab --reservation-affinity=any

Note: The bucket name for the log files as well as the location of the start-up script are added to the metadata: --metadata=^,@^lab-logs-bucket=gs://gsc-gce-challengelab-iar-cli2 --metadata-from-file startup-script='GCS&GCE_Challenge_Lab/start-up_script.sh'


Clean up
- gcloud projects delete gsc-gce-challengelab-cli2
