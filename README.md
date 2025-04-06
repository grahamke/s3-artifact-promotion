# 🚀 S3 Artifact Promotion · GitHub Actions Workflow

![License](https://img.shields.io/github/license/grahamke/s3-artifact-promotion)
![Build](https://img.shields.io/github/actions/workflow/status/grahamke/s3-artifact-promotion/promote-artifact.yml?branch=main)

> ⚠️ **This project is under active development.** Interfaces and behavior may change until v1.0.0 is released.

A reusable GitHub Actions workflow for promoting versioned build artifacts to an S3 bucket using secure AWS OIDC authentication. Automatically archives previous versions and promotes the new one as `latest/`.


### ✅ What it does:
- Uploads a versioned artifact to a `myapp/latest/` path in your bucket (filename includes version)
- If a previous artifact exists in `latest/`, it is archived to `archive/myapp/latest/`
- Keeps artifact versioning clean, consistent, and rollback-friendly
- Requires no long-lived AWS credentials — uses GitHub OIDC + IAM role

---

## 🔧 Usage

### 📥 Inputs

| Input             | Description                                                       | Required |
|-------------------|-------------------------------------------------------------------|----------|
| `app_name`        | The name of the application or artifact (e.g. `myapp`)            | ✅       |
| `version`         | Version label used in the versioned S3 path (e.g. `1.0.0`)        | ✅       |
| `artifact_path`   | Path to the built artifact file (e.g. `./build/myapp-1.0.0.jar`)  | ✅       |
| `bucket_name`     | Name of the S3 bucket to promote to (e.g. `my-artifact-store`)    | ✅       |
| `aws_region`      | AWS region where the S3 bucket and IAM role exist (e.g. `us-east-1`) | ✅    |
| `role_to_assume`  | ARN of the IAM role configured for GitHub OIDC access             | ✅       |


### Referencing the Action

In your project repo, call the reusable workflow:

```yaml
jobs:
  promote:
    uses: grahamke/s3-artifact-promotion/.github/workflows/promote-artifact.yml@main
    with:
      app_name: myapp
      version: 1.2.3
      artifact_path: ./build/myapp-1.2.3.jar
      bucket_name: ${{ vars.S3_BUCKET_NAME }}
      aws_region: us-east-1
      role_to_assume: ${{ secrets.ROLE_TO_ASSUME }}
