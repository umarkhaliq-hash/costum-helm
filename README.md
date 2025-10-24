# PrestaShop Helm Chart

A simple Helm chart for deploying PrestaShop e-commerce application with MySQL database on Kubernetes.

## Quick Start

### 1. Install the Application
```bash
helm install newapp prestashop/ --values prestashop/values.yaml
```

### 2. Check Pod Status
```bash
kubectl get pods
```
Wait until both pods show `1/1 Running`:
- `mysql-deployment-xxx` 
- `prestashop-xxx`

### 3. Access the Application

#### Option A: Port Forwarding (Recommended)
```bash
kubectl port-forward service/prestashop 8095:80
```
Then open: http://localhost:8095

#### Option B: NodePort
```bash
minikube service prestashop --url
```
Access via the provided URL (port 30095)

## Configuration

### Default Settings
- **PrestaShop Image**: `prestashop/prestashop:8.1`
- **MySQL Image**: `mysql:5.7`
- **NodePort**: `30095`
- **Admin Email**: `admin@prestashop.com`
- **Admin Password**: `admin123`

### Environment Variables
- `PS_INSTALL_AUTO=1` - Automatic installation
- `PS_DEMO_MODE=0` - No demo data
- `PS_ENABLE_SSL=0` - SSL disabled
- `PS_DOMAIN=localhost:8095` - Domain for redirects

## Troubleshooting

### Installation Taking Long
PrestaShop installation can take 3-5 minutes. Monitor progress:
```bash
kubectl logs -f prestashop-xxx
```
Look for: `* Almost ! Starting web server now`

### Port Forwarding Issues
If port 8095 is busy, use different port:
```bash
kubectl port-forward service/prestashop 8096:80
```

### Pod Not Ready
Check pod status and logs:
```bash
kubectl describe pod prestashop-xxx
kubectl logs prestashop-xxx
```

## Files Structure
```
prestashop/
├── templates/
│   ├── deployment.yaml    # PrestaShop deployment
│   ├── service.yaml       # Services & MySQL deployment
│   └── configmap.yaml     # Configuration data
├── values.yaml            # Configuration values
├── Chart.yaml             # Chart metadata
└── README.md             # This file
```

## Upgrade
```bash
helm upgrade newapp prestashop/ --values prestashop/values.yaml
```

## Uninstall
```bash
helm uninstall newapp
```

## Access Credentials
- **Frontend**: http://localhost:8095
- **Admin Panel**: http://localhost:8095/admin
- **Username**: admin@prestashop.com
- **Password**: admin123