# uniplace-ai

UniPlace's AI service: a FastAPI app for the general and agent chatbots, the voice assistant, RAG search, community
search and moderation, contract and room recommendations, payment summaries and operations monitoring. It was split out
of [IBM-1st-Project](https://github.com/Dev-Forge-Group/IBM-1st-Project) (`AI/`) with its history.

The UniPlace backend (Spring) is its only caller. The endpoints and the contract version are in
[docs/AI_API_CONTRACT.md](docs/AI_API_CONTRACT.md), which this repository owns; change it here, together with the
service. The service does not connect to UniPlace's MySQL: what it needs from business data goes through the backend's
`AiDataContract`.

## Run locally

```bash
cp .env.example .env    # then fill in the keys
docker compose up -d    # the vector store: Milvus with etcd and MinIO
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

The container image runs the same app (`uvicorn app.main:app` on port 8000).

## Image

The Image workflow runs the REST smoke tests and pushes `uniplace-ai:<commit>` and `uniplace-ai:main` to Amazon ECR on
each push to `main`. It needs the `AWS_ROLE_ARN`, `AWS_REGION` and `AWS_ACCOUNT_ID` secrets, and the AWS role must trust
this repository (`repo:Dev-Forge-Group/uniplace-ai:*`).

The cluster runs `uniplace-ai:latest` (IBM-1st-Project `deploy/k8s/ai`), which IBM-1st-Project's `dev` pipeline still
pushes. Move `latest` here only by running the Image workflow by hand with "promote to latest", once the UniPlace
backend deployed with it matches this service.
