# Premier exercise
Objectifs: Ajouter votre VM Windows 2016 au inventaire tower

- Ouvrez tower et connectez-vous
- Allez dans l'onglet "Inventories" et cliquez sur "+ADD" et "Inventory"
- Ajoutez les informations de votre VM:
-- Nom - votre nom de l'inventaire (group de VMs), p.ex "MesVMs" 
-- laissez l'organization Default
-- Allez sur le sub-onglet "Hosts" et cliquez sur "+ADD HOST"
-- Remplissez les details de la VM: 
--- Hostname: on n'a pas de DNS mais on va ajouter ici le nom "MaVMWindows2016"
--- Dans le box "Variables", au-dessous de "---" on ajoute une nouvelle ligne qui va dire 
'---
ansible_host: 40.86.222.127
ansible_user" MON_USERNAME'
--- Si l'on a oublié certains details de la VM on peut toujours voir le "Automation Script" qui a toutes les variables qu'on a utilisé pour créer notre VM dans Azure.
--- On sauvegarde le Host avec "Save""
- Ajoutez le mot de passe pour votre VM dans le menu de Tower "Settings" puis "Credentials"
-- Cliquez sur "+ADD" et remplissez les information des la nouvelle "credential"
--- Name: "Mon Mot De Passe Windows 2016"
--- Credential Type: "Machine"
--- Organization: "default"
--- Username: "MON_USERNAME", celui que vous avez configuré au demarrage de la VM dans azure
--- Password: Le password que vous avez configuré au demarrage de la VM dans azure
--- Cliquez sur Save	

