# To fetch updated code
```
git fetch upstream
git checkout master  # Ensure you’re on the main branch
git merge upstream/master  # Merge the latest changes

npm install -g pnpm
pnpm install
export NODE_OPTIONS="--max-old-space-size=8192" && pnpm build:docker
```


# To build and push the image to AWS
```
docker tag n8nio/n8n:local 122610520345.dkr.ecr.ap-south-1.amazonaws.com/n8n:latest
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 122610520345.dkr.ecr.ap-south-1.amazonaws.com
docker push 122610520345.dkr.ecr.ap-south-1.amazonaws.com/n8n:latest
```


# Deploy workflow image
```
aws configure --profile fnshift
aws ecr get-login-password --profile fnshift --region ap-south-1 | docker login --username AWS --password-stdin 122610520345.dkr.ecr.ap-south-1.amazonaws.com
docker pull 122610520345.dkr.ecr.ap-south-1.amazonaws.com/n8n:latest
docker run -d --name n8n --env-file .env -p 127.0.0.1:5678:5678  -e N8N_ENCRYPTION_KEY="GdZu8SWNVTz30FhvrdrgIKaFb4u8AfLk" -v n8n_data:/home/node/.n8n 122610520345.dkr.ecr.ap-south-1.amazonaws.com/n8n:latest
docker logs --follow n8n
```
