##### Retrieve an authentication token and authenticate your Docker client to your registry

    aws ecr get-login-password --region REGION | docker login --username AWS --password-stdin AWS_ID.dkr.ecr.REGION.amazonaws.com

##### Build your Docker image using the following command
    
    docker build -t my-app .

##### After the build completes, tag your image so you can push the image to this repository

    docker tag my-app:latest AWS_ID.dkr.REGION.amazonaws.com/my-app:latest

##### Run the following command to push this image to your newly created AWS repository

    docker push AWS_ID.dkr.ecr.REGION.amazonaws.com/my-app:latest

##### generate secret file
    touch .docker
    cd .docker/config.json

##### access minikube console

    minikube ssh

##### login to docker and generate config.json file

    docker login --username AWS -p PASSWORD IMAGE_URI

##### copy config.json file from Minikube to my host and encrypt in base64

    scp -i $(minikube ssh-key) docker@$(minikube ip):.docker/config.json .docker/config.json
    cat .docker/config.json | base64

##### create docker login secret from config.json file

    kubectl create secret generic my-registry-key \
    --from-file=.dockerconfigjson=.docker/config.json \
    --type=kubernetes.io/dockerconfigjson

    kubectl create secret generic my-registry-key --from-file=.dockerconfigjson=.docker/config.json --type=kubernetes.io/dockerconfigjson

    kubect get secret

##### create docker login secret with login credentials

    kubectl create secret docker-registry my-registry-key \
    --docker-server=https://private-repo \
    --docker-username=user \
    --docker-password=pwd

    kubectl create secret docker-registry my-registry-key --docker-server=https://private-repo --docker-username=user --docker-password=pwd

