# Using ingress with SSL/TLS

The WSO2 Kubernetes Pipeline resource uses the NGINX Ingress Controller maintained by Kubernetes. 
It is possible to use SSL/TLS by adding a certificate to be used with the ingress controller.
This could be done using the following methods
- [Default SSL certificate](#default-ssl-certificate)
- [Add individual certificates](#add-individual-certificates)

## Default SSL certificate

Refer to [NGINX Ingress Controller user guide](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#default-ssl-certificate) on how to configure a default certificate.

## Add individual certificates

To add individual certificates to the each ingress endpoint,

1. Create secrets for each endpoint containing the certificate and the private key as mentioned in the [NGINX Ingress Controller user guide](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#tls-secrets).
2. Add the following content to your values.yaml with the secret and hostname
 
 > Replace `example.com`, `my-tls-cert` with your domain name and secret name respectively.


```yaml
jenkins:
  ingress:
    host: jenkins.example.com
    tls:
      - secretName: my-tls-cert
        hosts:
          - example.com

spinnaker:
  ingress:
    host: spinnaker.example.com
    tls:
      - secretName: -tls
        hosts:
          - example.com
  ingressGate:
    host: gate.spinnaker.example.com
    tls:
     - secretName: -tls
       hosts:
         - example.com

kibana:
  ingress:
    hosts:
      - kibana.example.com
    tls:
      - hosts:
          - example.com
        secretName: my-tls-cert

prometheus-operator:
  grafana:
    ingress:
      hosts:
      - grafana.example.com
      tls:
        - hosts:
            - example.com
          secretName: my-tls-cert
```
