# Premier exercise
Objectifs: Ajouter votre VM Windows 2016 au inventaire tower

- Ouvrez tower et connectez-vous
## Gestion Inventaire
- Allez dans l'onglet "Inventories" et cliquez sur **+ADD** et **Inventory**
- Ajoutez les informations de votre VM:
- **Nom** - votre nom de l'inventaire (group de VMs), p.ex "MesVMs" 
- laissez l'organization *Default*
- Allez sur le sub-onglet "Hosts" et cliquez sur **+ADD HOST**

Dans ce nouveau menu, remplissez les details de la VM: 
- **Hostname**: on n'a pas de DNS mais on va ajouter ici le nom "MaVMWindows2016"
- Dans le box **Variables**, au-dessous de "---" on ajoute des variables comme suit 
```
---
ansible_host: 40.86.222.127
ansible_user" MON_USERNAME
ansible_port: 5985
ansible_connection: winrm
ansible_winrm_transport: basic
```
- Si l'on a oublié certains details de la VM on peut toujours voir le "Automation Script" qui a toutes les variables qu'on a utilisé pour créer notre VM dans Azure.
On sauvegarde le Host avec **Save**

## Gestion credentiaux
Ajoutez le mot de passe pour votre VM dans le menu de Tower **Settings** puis **Credentials**
Ensuite, cliquez sur **+ADD** et remplissez les information des la nouvelle "credential"
- **Name**: "Mon Mot De Passe Windows 2016"
- **Credential Type**: "Machine"
- **Organization**: "default"
- **Username**: "MON_USERNAME", celui que vous avez configuré au demarrage de la VM dans azure
- **Password**: Le password que vous avez configuré au demarrage de la VM dans azure
Cliquez sur Save	

## Execution de commande de test
On reviens aux onglet **Inventories**, puis on va séléctionner "MesVMs", puis le sous-onglet **Hosts** et on va séléctionner "MaVMWindows2016"

Si on regarde dans la moitié inférieure de l'écran, du coté droit, on va voir un bouton qui dit **Run Commands**. On va sélectionner le item "MaVMWindows2016" puis on va cliquer sur **Run Commands**

Dans la nouvelle ecran *Execute Command* on va séléctionner le **Module** "win_ping" (dans le pliable "choose a module"), puis on va choisir le **Machine Credential** qu'on a crée "Mon Mot De Passe Windows 2016" pius on va cliquer sur **Launch**

Si tout va bien, on va voir le resultat de de l'éxecution comme une nouvelle job (visible dans l'onglet *Jobs*) et avec un resultat comme celui-ci
```
MaVMWindows2016 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```


## Execution de playbook de test
Maintenant on va importer notre premier playbook depuis le depot du workshop https://github.com/RHMTLSAS/ansiblewindowsworkshop/
On va aller au onglet **Projects** puis on clique sur **+ADD**
- dans **Name** on écrit "Ansible Windows Workshop"
- **Organization**: "default"
- **SCM Type** git
- **SCM URL** https://github.com/RHMTLSAS/ansiblewindowsworkshop/
- On active *Update on Launch*
- puis **Save**

Ensuite on va configurer une Job Template, c'est là qu'on va choisir quel **Playbook** va s'executer sur quel **Inventory** avec les **Credentials** nécessaires. 
Dans ce cas, on va executer le playbook suivant (https://github.com/RHMTLSAS/ansiblewindowsworkshop/blob/master/test_playbooks/filetest.yml), qui va simplemen créer un fichier "hello world" dans C:/temp/file
On va allors sur l'onglet **Templates** puis **+ADD** et **Job Template**
- dans **Name** on écrit *"Create file in temp folder in Windows"*
- **Job Type**: Run
- **Inventory**: "MesVMs"
- **Project**: *Ansible Windows Workshop* (le nom du Projet avec le depot Git du workshop)
- **Playbook**: dépliez la liste et sélectionnez *test_playbook/filetest.yml*
- **Credential**: "Mon Mot De Passe Windows 2016"
- Laissez le reste par défault, puis cliquez sur **Save**

Alors on est pret pour executer notre premier playbook template. On va dans l'onglet **Templates**, et on va chercher dans la list le job template *"Create file in temp folder in Windows"*.
On va éxecuter la template immediatement via le icon du "rocket" à droite.
Si tout marche bien, on devrait voir la correcte éxécution du playbook avec un message comme celui-ci:
```

PLAY [test windowsRM and win2016] **********************************************
18:39:36
TASK [Gathering Facts] *********************************************************
18:39:36
ok: [MaVMWindows2016]
TASK [Put a file] **************************************************************
18:39:42
ok: [MaVMWindows2016]
PLAY RECAP *********************************************************************
18:39:46
MaVMWindows2016            : ok=2    changed=0    unreachable=0    failed=0   

```
