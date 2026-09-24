# Digitech GitOps Hosting Platform 🚀

A self-hosted, fully automated GitOps platform for deploying and managing static websites, digital menus, and web applications. Built for the Digitech agency to streamline client onboarding and hosting on an Oracle Cloud ARM architecture.

## 🏗️ Architecture & Tech Stack

This project utilizes a modern, containerized monorepo approach:

* **Infrastructure:** Oracle Cloud (Always Free ARM VPS)
* **Orchestration:** Kubernetes (K3s)
* **CI/CD:** GitHub Actions (Automated multi-arch builds & dynamic manifests)
* **Containerization:** Docker & Docker Buildx (QEMU for `linux/arm64`)
* **Registry:** GitHub Container Registry (GHCR)
* **Routing & SSL:** Traefik Ingress + Cert-Manager (Let's Encrypt)
* **Management:** Portainer CE

## ⚙️ How It Works (The Automation)

The repository uses a **Monorepo** structure. The GitHub Actions workflow (`.github/workflows/deploy.yml`) is configured to listen for changes in the `sites/` directory. 

When a commit is pushed:
1. The workflow scans the `sites/` directory for all client folders.
2. It dynamically generates a `Dockerfile` for each site.
3. It builds a `linux/arm64` Docker image and pushes it to GHCR.
4. It generates Kubernetes manifests (Deployment, Service, and Ingress) on the fly.
5. It applies the manifests to the K3s cluster.
6. Cert-Manager automatically requests and issues a Let's Encrypt TLS certificate for the generated `.nip.io` domain.

## 📁 Repository Structure

\`\`\`bash
.
├── .github/
│   └── workflows/
│       └── deploy.yml      # The GitOps CI/CD pipeline
├── sites/
│   ├── client-a/           # Client A's website
│   │   ├── index.html
│   │   └── css/
│   └── client-b/           # Client B's website
│       ├── index.html
│       └── js/
└── README.md
\`\`\`

## 🚀 How to Add a New Site

Deploying a new client site requires **zero server configuration**:

1. Create a new folder inside the `sites/` directory (e.g., `sites/new-client`).
2. Add your HTML/CSS/JS files (must include an `index.html`).
3. Commit and push the changes:
   \`\`\`bash
   git add .
   git commit -m "Add new client site"
   git push
   \`\`\`
4. **First-time only:** Go to your GitHub Profile -> Packages, find the newly created container package, and change its visibility from Private to **Public**.
5. The site will be instantly available and secured at `https://new-client-YOUR.SERVER.IP.nip.io`.

## 📊 Infrastructure Management

The K3s cluster and deployed containers are managed visually via **Portainer**. 
Access the Portainer dashboard at: `https://portainer-YOUR.SERVER.IP.nip.io`


You can watch these 2 public live servers at :
🟢demo-menu:https://demo-menu-130.61.72.72.nip.io/
🟢pelatis-2:https://pelatis-2-130.61.72.72.nip.io/
