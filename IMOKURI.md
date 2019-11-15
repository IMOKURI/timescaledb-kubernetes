# Memo by imokuri

## install

```
helm upgrade --install sugi-timescaledb --namespace sugi-timescaledb charts/timescaledb-single -f charts/timescaledb-single/values.yaml
```

install log

```
Release "sugi-timescaledb" does not exist. Installing it now.
NAME:   sugi-timescaledb
LAST DEPLOYED: Fri Nov 15 17:29:43 2019
NAMESPACE: sugi-timescaledb
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                      DATA  AGE
sugi-timescaledb-patroni  1     1s
sugi-timescaledb-scripts  4     1s

==> v1/Pod(related)
NAME                READY  STATUS   RESTARTS  AGE
sugi-timescaledb-0  0/1    Pending  0         1s

==> v1/Role
NAME              AGE
sugi-timescaledb  1s

==> v1/RoleBinding
NAME              AGE
sugi-timescaledb  1s

==> v1/Secret
NAME                          TYPE               DATA  AGE
sugi-timescaledb-certificate  kubernetes.io/tls  2     1s
sugi-timescaledb-passwords    Opaque             3     1s

==> v1/Service
NAME                      TYPE          CLUSTER-IP     EXTERNAL-IP  PORT(S)         AGE
sugi-timescaledb          LoadBalancer  10.97.138.100  <pending>    5432:31241/TCP  1s
sugi-timescaledb-replica  ClusterIP     None           <none>       5432/TCP        1s

==> v1/ServiceAccount
NAME              SECRETS  AGE
sugi-timescaledb  1        1s

==> v1/StatefulSet
NAME              READY  AGE
sugi-timescaledb  0/3    1s


NOTES:

TimescaleDB can be accessed via port 5432 on the following DNS name from within your cluster:
sugi-timescaledb.sugi-timescaledb.svc.cluster.local

To get your password for superuser run:

    # superuser password
    PGPASSWORD_POSTGRES=$(kubectl get secret --namespace sugi-timescaledb sugi-timescaledb-passwords -o jsonpath="{.data.postgres}" | base64 --decode)

    # admin password
    PGPASSWORD_ADMIN=$(kubectl get secret --namespace sugi-timescaledb sugi-timescaledb-passwords -o jsonpath="{.data.admin}" | base64 --decode)

To connect to your database, chose one of these options:

1. Run a postgres pod and connect using the psql cli:
    # login as superuser
    kubectl run -i --tty --rm psql --image=postgres \
      --env "PGPASSWORD=$PGPASSWORD_POSTGRES" \
      --command -- psql -U postgres \
      -h sugi-timescaledb.sugi-timescaledb.svc.cluster.local postgres

    # login as admin
    kubectl run -i --tty --rm psql --image=postgres \
      --env "PGPASSWORD=$PGPASSWORD_ADMIN" \
      --command -- psql -U admin \
      -h sugi-timescaledb.sugi-timescaledb.svc.cluster.local postgres

2. Directly execute a psql session on the master node

   MASTERPOD="$(kubectl get pod -o name --namespace sugi-timescaledb -l release=sugi-timescaledb,role=master)"
   kubectl exec -i --tty --namespace sugi-timescaledb ${MASTERPOD} -- psql -U postgres

```
