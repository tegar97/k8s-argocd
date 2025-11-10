# Cert-Manager Setup

## Prerequisites

Cert-manager harus sudah terinstall di cluster. Jika belum:

```bash
# Install cert-manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.yaml

# Verify installation
kubectl get pods -n cert-manager
```

## Deploy ClusterIssuer

1. **Edit email** di `cluster-issuer.yaml` (line 9 dan 26)
2. Apply ClusterIssuer:

```bash
kubectl apply -f cert-manager/cluster-issuer.yaml
```

3. Verify:

```bash
# Check ClusterIssuer status
kubectl get clusterissuer
kubectl describe clusterissuer letsencrypt-prod
```

## How It Works

- **letsencrypt-prod**: Untuk production, rate limit ketat
- **letsencrypt-staging**: Untuk testing, rate limit lebih longgar

Ingress sudah dikonfigurasi dengan annotation:
```yaml
cert-manager.io/cluster-issuer: letsencrypt-prod
```

Cert-manager akan otomatis:
1. Detect Ingress dengan TLS config
2. Request certificate dari Let's Encrypt
3. Store certificate di Secret (contoh: `my-app-prod-tls`)
4. Auto-renew sebelum expire

## Troubleshooting

```bash
# Check certificate status
kubectl get certificate -A

# Check certificate request
kubectl get certificaterequest -A

# Check cert-manager logs
kubectl logs -n cert-manager -l app=cert-manager
```
