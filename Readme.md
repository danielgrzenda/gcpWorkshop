# Introduction to Cloud Computing 

## Helpful Commands 

whoami  
hostname  
curl api.ipify.org; echo ""  
gcloud config list  
gcloud config set compute/region us-central1  
gcloud config set compute/zone us-central1-b  
gcloud beta billing accounts list  

## Making a Bucket

gsutil ls  
gsutil mb -l us-central1 gs://gcpworkshoptest  
gsutil cp test.txt gs://gcpworkshoptest  
gsutil rm -r gs://gcpworkshoptest  

## Launching a Web Server (Hello World)

### Setup 

gcloud config set compute/region us-central1  
gcloud config set compute/zone us-central1-b  

### Server

gcloud compute instances create myvm  
gcloud compute machine-types list  
gcloud compute machine-types list | grep f1-micro | grep us-central1  
gcloud compute machine-types list --filter "NAME:f1-micro AND ZONE:us-central1-b"  
gcloud compute instances delete myvm  
gcloud compute addresses create mystaticip  
gcloud compute instances create myvm --address mystaticip   
 
### SSH

gcloud compute ssh myvm  
whoami  
hostname  
curl api.ipify.org; echo ""  
#sudo apt update  
#sudo apt upgrade  
sudo apt update && sudo apt -y install apache2  
echo '\<!doctype html\>\<html\>\<body\>\<h1\>Hello World!\</h1\>\</body\>\</html\>' | sudo tee /var/www/html/index.html  
gcloud compute firewall-rules create ingress-tcp-port80-from-all \  
    --action allow \  
    --direction ingress \  
    --rules tcp:80 \  
    --source-ranges 0.0.0.0/0  
    --priority 1000 \  
    --target-tags http-server  
gcloud compute instances add-tags myvm --tags http-server  

### Clean-up 

gcloud compute instances delete myvm  
gcloud compute addresses delete mystaticip  
gcloud compute firewall-rules delete ingress-tcp-port80-from-all   
