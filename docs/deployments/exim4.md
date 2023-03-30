# Mail

!!! success "Prerequisites"

    [exim4-helm](https://github.com/mb-wali/exim4-helm)

    A Helm chart to provide a exim4 deployment, exim4 (MTA) running as a smarthost.

## Deploy

### Add & update helm chart

```bash
helm repo add exim4 https://mb-wali.github.io/exim4-helm
helm repo update
```

### Install

```bash
# replace the secrets with yours
helm install exim4 --set secrets.EXIM_SMARTHOST='localhost',secrets.EXIM_PASSWORD='passw0rd',secrets.EXIM_ALLOWED_SENDERS='*' exim4/exim4 --namespace mail --create-namespace --wait
```

### Debugging

Once the pod is running

```bash
# execute shell 
kubectl exec -it exim4-6ff546fb9f-ff47m -- bash

# send a test mail
echo "This is test" | mail -s "The subject" receiver@myhost.com -aFrom:sender@myhost.com
```

### Usage

Use your deployed exim4 to send mails, 
e.g. connect from a another service.

```bash
SMTP_HOST=exim4.mail.svc.cluster.local
```
