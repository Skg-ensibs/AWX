Lorsque AWX est installé faut créer un project qui lui dit ou se trouve les .yml : 

```
awx project create \
  --name "SIMON" \
  --organization "Default" \
  --scm_type "" \
  --local_path "PATH"
```


Commande pour créer les jobs templates en cli : 
```
awx job_template create \
  --name "<Nom du template>" \
  --inventory "<Nom de l'inventaire>" \
  --project "<Nom du projet>" \
  --playbook "<fichier.yml>" \
  --extra-vars '{"clé1": "valeur1", "clé2": "valeur2"}'
```

Perso sur mon je mettais ça :
```
awx --conf.host http://127.0.0.1:8080 --conf.token yPLLeIEXMVoEImY2Qh0ixIG78puxQA job_template create \
  --name "<Nom du template>" \
  --inventory "<Nom de l'inventaire>" \
  --project "<Nom du projet>" \
  --playbook "<fichier.yml>" \
  --extra-vars '{"clé1": "valeur1", "clé2": "valeur2"}'
```

  Maintenant ce qui suit c'est les tous ceux à lancer :

```
#!/bin/bash

AWX_HOST="http://127.0.0.1:8080"
AWX_TOKEN="<TON_TOKEN>"
INVENTORY="TPE-Inventaire"
PROJECT="SIMON"

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Déploiement partage fichiers" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "deploy_file_share.yml" \
  --extra-vars '{"share_name": "documents", "share_path": "/srv/documents", "allowed_users": ["jean", "julie"], "access_mode": "rw"}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Ajout utilisateur" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "add_user.yml" \
  --extra-vars '{"username": "jdupont", "full_name": "Jean Dupont", "groups": ["sudo", "smb"], "create_home": true}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Suppression utilisateur" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "delete_user.yml" \
  --extra-vars '{"username": "lucas", "remove_home": true}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Modification permissions utilisateur" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "modify_user_permissions.yml" \
  --extra-vars '{"username": "alice", "add_groups": ["smb", "dev"], "remove_groups": ["test", "guests"]}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Création partage réseau" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "create_network_share.yml" \
  --extra-vars '{"share_name": "partage_client", "share_path": "/srv/partage_client", "allowed_users": ["jean", "julie"], "protocol": "nfs", "read_only": true}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Mise à jour système" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "system_update.yml" \
  --extra-vars '{"os_family": "debian", "perform_upgrade": true, "reboot_if_required": false}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Déploiement serveur web" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "deploy_web_server.yml" \
  --extra-vars '{"web_server": "apache", "document_root": "/var/www/my_site", "server_name": "site.tpe.local", "enable_ssl": true}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Sauvegarde serveur" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "backup_server.yml" \
  --extra-vars '{"backup_sources": ["/home", "/var/lib/mysql"], "backup_destination": "/mnt/backup", "compress_backup": true, "retention_days": 14}'

awx --conf.host $AWX_HOST --conf.token $AWX_TOKEN job_template create \
  --name "Réinitialisation mot de passe" \
  --inventory "$INVENTORY" \
  --project "$PROJECT" \
  --playbook "reset_password.yml" \
  --extra-vars '{"username": "jean", "new_password": "Ch@ngeMe123!"}'

```
