# Hi, I'm Aadil Agwan 👋

**DevOps Engineer** | **Infrastructure as Code (IaC)** | **CI/CD Automation** | **Cloud Native** ☁️

🏠 *Based in Mumbai, India* | 🔗 [LinkedIn](https://www.linkedin.com/in/aadil-agwan/) | 📧 agwan96@gmail.com

---

## 🎯 About Me

I'm a **Senior DevOps Engineer** with **5+ years** of hands-on experience architecting and automating cloud infrastructure at scale. Passionate about building resilient, automated systems using modern DevOps practices, Infrastructure as Code, and containerization. Currently focused on optimizing CI/CD pipelines, Kubernetes orchestration (k3s, Rancher), and GitOps workflows.

- 🏢 Working with **AWS, Terraform, Kubernetes, Docker, GitHub Actions**
- 🎓 Always learning and experimenting with new cloud-native technologies
- 🏠 Homelab enthusiast: Running Pi Cluster for Kubernetes experimentation
- 🔄 Strong advocate for **automation-first** and **Infrastructure-as-Code** practices

---

## 💻 Core Competencies

### Infrastructure & Cloud
![AWS](https://img.shields.io/badge/-AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Terraform](https://img.shields.io/badge/-Terraform-623CE4?style=flat&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat&logo=docker&logoColor=white)

### CI/CD & Automation
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![GitLab CI](https://img.shields.io/badge/-GitLab%20CI-FCA121?style=flat&logo=gitlab&logoColor=white)
![Jenkins](https://img.shields.io/badge/-Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![ArgoCD](https://img.shields.io/badge/-ArgoCD-EF7B4D?style=flat&logo=argo&logoColor=white)

### Tools & Technologies
![Helm](https://img.shields.io/badge/-Helm-0F1419?style=flat&logo=helm&logoColor=white)
![Prometheus](https://img.shields.io/badge/-Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/-Grafana-F2CC0C?style=flat&logo=grafana&logoColor=black)
![Linux](https://img.shields.io/badge/-Linux-FCC624?style=flat&logo=linux&logoColor=black)
![Python](https://img.shields.io/badge/-Python-3776AB?style=flat&logo=python&logoColor=white)
![Shell Scripting](https://img.shields.io/badge/-Bash-4EAA25?style=flat&logo=gnu-bash&logoColor=white)

---

## 🚀 Key Projects

### 1. **DevOps Sandbox** 🏗️
   *Comprehensive CI/CD automation with GitOps principles*
   
   - **GitHub Actions Workflows**: Automated testing, building, and deployment pipelines
   - **IaC with Terraform**: AWS infrastructure provisioning and management
   - **Kubernetes Deployment**: Multi-stage deployments with Helm charts
   - **GitOps Pipeline**: ArgoCD for declarative, git-driven infrastructure updates
   
   **Key Features**:
   - Auto-scaling EC2 instances with Terraform
   - Docker image building and registry management
   - Automated security scanning in CI/CD pipeline
   - Environment-specific configurations (dev/staging/prod)
   
   [→ Explore Repository](https://github.com/aadil96/devops)

### 2. **Pi Cluster - Kubernetes Homelab** 🍓
   *Learning and experimentation platform for Kubernetes orchestration*
   
   - **k3s Lightweight Distribution**: Perfect for edge computing and learning
   - **Infrastructure Automation**: Automated cluster setup and management
   - **Rancher Integration**: Visual management and monitoring
   - **WSL2 Networking**: Advanced networking configurations for development
   - **Persistent Storage**: Local storage provisioning and management
   
   [→ Explore Repository](https://github.com/aadil96/pi-cluster)

### 3. **Dotfiles & Configuration Management** ⚙️
   *Version-controlled development environment setup*
   
   - Neovim configuration with CLI-focused workflow
   - Shell scripting for environment automation
   - IaC approach to personal infrastructure
   
   [→ Explore Repository](https://github.com/aadil96/dotfiles-rio)

---

## 📋 CI/CD Pipeline Examples

### GitHub Actions Workflow - Build & Deploy
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker Image
        run: docker build -t ${{ secrets.ECR_REGISTRY }}/app:${{ github.sha }} .
      
      - name: Push to ECR
        run: docker push ${{ secrets.ECR_REGISTRY }}/app:${{ github.sha }}
      
      - name: Update Helm Values
        run: |
          sed -i 's|IMAGE_TAG|${{ github.sha }}|g' helm/values.yaml
          git config user.name 'github-actions'
          git commit -am 'chore: update image tag'
          git push
      
      - name: Deploy with ArgoCD
        run: |
          argocd app sync production --auth-token ${{ secrets.ARGOCD_TOKEN }}
```

---

## 🏗️ Infrastructure as Code Examples

### Terraform - AWS EKS Cluster Setup
```hcl
# AWS EKS Cluster with Terraform
resource "aws_eks_cluster" "main" {
  name            = "production-cluster"
  version         = "1.27"
  role_arn        = aws_iam_role.eks_cluster.arn
  
  vpc_config {
    subnet_ids              = aws_subnet.private[*].id
    endpoint_private_access = true
    endpoint_public_access  = false
  }
  
  depends_on = [aws_iam_role_policy_attachment.eks_cluster_policy]
}

# Auto Scaling Group for Node Groups
resource "aws_eks_node_group" "main" {
  cluster_name    = aws_eks_cluster.main.name
  node_group_name = "main-node-group"
  node_role_arn   = aws_iam_role.eks_nodes.arn
  subnet_ids      = aws_subnet.private[*].id
  
  scaling_config {
    desired_size = 3
    max_size     = 10
    min_size     = 1
  }
}
```

### Kubernetes Manifests - GitOps Ready
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    name: production
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-service
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api-service
  template:
    metadata:
      labels:
        app: api-service
    spec:
      containers:
      - name: api
        image: registry.example.com/api:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 10
```

---

## 🛠️ Tools & Technologies

### DevOps & Infrastructure
- **Container Orchestration**: Kubernetes (k3s), Rancher, Docker Compose
- **IaC Tools**: Terraform, Helm, CloudFormation
- **CI/CD Platforms**: GitHub Actions, GitLab CI, Jenkins
- **Configuration Management**: Ansible, Vagrant
- **Monitoring & Logging**: Prometheus, Grafana, ELK Stack, CloudWatch

### Cloud Platforms
- **AWS Services**: EC2, S3, RDS, Lambda, EKS, IAM, VPC, CloudFormation
- **Other Clouds**: Explored GCP and Azure architectures

### Languages & Scripting
- **Python** - Automation scripts and tooling
- **Bash/Shell** - System administration and DevOps scripting
- **YAML** - Kubernetes manifests, Terraform configs, CI/CD workflows
- **HCL** - Terraform infrastructure definitions

---

## 📚 Featured Learning Resources

- 📖 **Lab Repository**: Experiment space for DevOps concepts and tools
- 🎯 **Hands-on Projects**: Real-world scenarios with AWS, Kubernetes, and CI/CD
- 💡 **Best Practices**: Infrastructure automation, security scanning, and deployment strategies

---

## 📊 GitHub Stats

<div align="center">
  
![aadil96's GitHub Stats](https://github-readme-stats.vercel.app/api?username=aadil96&show_icons=true&theme=dark&hide_border=true)

![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=aadil96&layout=compact&theme=dark&hide_border=true)

</div>

---

## 🎓 Certifications & Learning

- 🏆 Hands-on experience with production Kubernetes clusters
- 🚀 AWS architecture and infrastructure automation
- 🔐 Security best practices in DevOps workflows
- 📊 Monitoring, logging, and observability patterns

---

## 🤝 Let's Connect!

I'm always open to discussing:
- Infrastructure automation and best practices
- Kubernetes and container orchestration
- CI/CD pipeline optimization
- Cloud-native architecture
- DevOps tooling and workflows

**Let's build something amazing together!** 🚀

---

<div align="center">
  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/aadil-agwan/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:agwan96@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/aadil96)

⭐ Feel free to explore my repositories and don't hesitate to reach out!

</div>
