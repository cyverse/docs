# TODO

!!! success "Prerequisites"


## Reindex RabbitMQ jobs

### access the vm where the RabbitMQ is installed.

```bash
ssh root@RABBITMQ_HOST
```

### Configure rabbitmqadmin

This step you have to do only once, if the rabbitmqadmin is not present.

```bash
mkdir adm
cd adm
wget http://localhost:15672/cli/rabbitmqadmin
chmod +x rabbitmqadmin
```

### Commands 

#### discover 

```bash
# check status
systemctl status rabbitmq-server.service -l

## add your password to a temp var
read -s PASSWORD && export PASSWORD

# list exchange
./rabbitmqadmin -V /cyverse/de list exchanges -u cyverse -p $PASSWORD

# publish the message to reindex all
./rabbitmqadmin publish -V /cyverse/de -u cyverse -p $PASSWORD exchange=de routing_key=index.all payload=""

# restart infosquito2
kubectl rollout restart deployment infosquito2 -n discover
# IF NOT deployed
# ./deploy.py -Bn discover -p infosquito2 -C
```

#### prod
```bash
## add your password to a temp var
read -s PASSWORD && export PASSWORD

# list
./rabbitmqadmin -V /tugraz/de list exchanges -u tugraz -p $PASSWORD

# publish the message to reindex all
./rabbitmqadmin publish -V /tugraz/de -u tugraz -p $PASSWORD exchange=de routing_key=index.all payload=""

```

