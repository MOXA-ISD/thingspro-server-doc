# ThingsPro Server on AWS

## Data Migration

## Quick and Easy Process (Requires version >= 2.7)

  1. Login to Main Server with key pair `EC2KeyName` and ip  `MainServerPublicIp`.
   For instance：`ssh -i thingspro-cs-oregon.pem ubuntu@34.212.156.174`

  2. Executing ommands with root privileges
  ```shell
  ubuntu@ip-172-99-10-174:/home/ubuntu# sudo su
  ```

  3. Go to `thingspro` folder
  ```shell
  root@ip-172-99-10-174:/home/ubuntu# cd thingspro
  ```

  4. run `migration-master.sh`
  ```shell
  root@ip-172-99-10-174:/home/ubuntu/thingspro# bash scripts/migration-master.sh 
  ```

## Migration and Upgrade Process ( upgrade to 2.7 )

### Prepare deployment on dev server

  1. Upload installation assets (ThingsProCS_20180501-001.tar.gz) to Jump Box (with ip `DevServerPublicIp`). For example:
  `rsync -av -e 'ssh -i thingspro-cs-oregon.pem' ThingsProCS_20180501-001.tar.gz ubuntu@34.212.156.174:/home/ubuntu`

  2. Login to Jump Box with key pair `EC2KeyName` and ip  `DevServerPublicIp`.
  For instance: `ssh -i thingspro-cs-oregon.pem ubuntu@34.212.156.174`

  3. Unzip ThingsProCS_20180501-001.tar.gz to `/home/ubuntu/thingspro`
  ```shell
  root@ip-172-99-10-174:/home/ubuntu# tar zxvf ThingsProCS_20180501-001.tar.gz -C thingspro
  ```

  4. Upload docker images to ECS
  **IMAGES_REPO** could be found in `ECSRepositoryUri`
  **VERSION** could be found in the filename `ThingsProCS_*20180501-001*.tar.gz`
  ```shell
  root@ip-172-99-9-41:/home/ubuntu/thingspro# make import-images
  root@ip-172-99-9-41:/home/ubuntu/thingspro# make push-images VERSION=20180501-001 STACK=demo IMAGES_REPO=656337455073.dkr.ecr.us-west-2.amazonaws.com/demo
  ```

  5. Modify deploy script for main server `scripts/aws-init-master.sh`
  ```shell
  export STACKNAME=demo
  export ECSRepositoryUri=656337455073.dkr.ecr.us-west-2.amazonaws.com/demo

  # ECR endpoint and region
  export IMAGE_REPO="$ECSRepositoryUri"
  export ECS_REGION=us-west-2

  # Version of images
  export VERSION=LATEST_VERSION_OF_IMAGES
  ```

  6. Create & Upload `deploy.tar.gz` to s3
   ```shell
   root@ip-172-99-9-41:/home/ubuntu/thingspro# make deploy.tar.gz
   root@ip-172-99-10-174:/home/ubuntu/thingspro# aws s3 cp deploy.tar.gz s3://s3.<STACKNAME>.thingspro
   ```