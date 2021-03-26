# AWX:
Deploying AWX on Kubernetes using AWX Operator.

1. Postgresql DB is used as an external DB option and all deployment files are located in Postgres folder.

2. <resource-name> is specified based on the name given in the `AWX.yaml` file
```yaml
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  tower_admin_user: awx
  tower_hostname: awx.local
  tower_ingress_type: Ingress
```

3. Postgresql configuration needs to be stored in the following form as a secret as per the documentation `<resourcename>-postgres-configuration` so our secret should be 
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: awx-postgres-configuration
  namespace: awx
stringData:
  host: <external ip or url resolvable by the cluster>
  port: <external port, this usually defaults to 5432>
  database: <desired database name>
  username: <username to connect as>
  password: <password to connect with>
type: Opaque
```

4. AWX CRD is modified to reflect this change 
```yaml
tower_postgres_configuration_secret:
    description: Secret where the database configuration can be found
    type: string
    default: awx-postgres-configuration 
```

5. The last resource to be deployed is the awx.yaml file

---

### Deployment steps:
