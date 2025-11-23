# Backup Commands

### Backup all resources declaratively
```
kubectl get all --all-namespaces -o yaml > all-deployed-servics.yaml
```

### Take snapshot of etcd using etcdctl (container based backup)
#### Note: Certificates are needed to authenticate to endpoint within etcd container)
```
ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /backup/etcd-snapshot.db
```

### Take snapshot of etcd using etcdutl (file based backup)
```
etcdutl backup \
  --data-dir /var/lib/etcd \
  --backup-dir /backup/etcd-backup
```

### Snapshot status 
```
etcdctl snapshot status /backup/etcd-snapshot.db \
  --write-out=table
```

### System restore via etcdutl
```
etcdutl snapshot restore /backup/etcd-snapshot.db \
  --data-dir /var/lib/etcd-restored
```

