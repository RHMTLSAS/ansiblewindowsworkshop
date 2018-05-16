# ansiblewindowsworkshop
Ansible Windows Workshop 

# Instructions pour se connecter à Azure 
Pre-requis: 
- Une subscription Azure (portal.azure.com), par exemple "Pay-as-you-go", capable de consommer des instances comme RHEL, ne pas incluses dans le "free trial"
- Configurer accès a notre compte via API, en ajoutant une "webapp" avec des droits d'écriture tel que expliqué ici https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal

## Comment récupérer les ID de notre compte azure:
On a besoin de 4 ID (example ci-bas):
- AZURE_CLIENT_ID=11111111-ff8c-4e1e-ab94-517778d11111
- AZURE_SECRET=11111111ZjOpJngNV0np0AtWhQXUjawbjt9ca111111=
- AZURE_TENANT=11111111-2f1a-492b-bb52-feed52111111
- AZURE_SUBSCRIPTION_ID=11111111-cf72-479a-80c8-f22c11111111

Les instructions se trouvent dans cette page: https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.6/html/managing_providers/cloud_providers#azure_providers , où c'est indiqué exactement dans quelle étape on peut récupérer les ID réquis pour l'accès via API documenté dans https://docs.microsoft.com/fr-FR/azure/azure-resource-manager/resource-group-create-service-principal-portal
- CLIENT_ID c'est le Application ID, similaire à un nom d'utilisateur, trouvé dans l'étape 2 de https://docs.microsoft.com/fr-FR/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-application-id-and-authentication-key
- SECRET c'est le "mot de passe" aléatoire generé dans la derniere étape 5 de https://docs.microsoft.com/fr-FR/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-application-id-and-authentication-key
- TENANT c'est le ID de locataire décrit dans https://docs.microsoft.com/fr-FR/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id
- SUBSCRIPTION c'est le ID de votre subscription, voir https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/

## Configuration du CLI
Téléchargez la dernière version du Azure CLI universel (Windows, Linux et MacOS)
https://docs.microsoft.com/fr-FR/cli/azure/install-azure-cli?view=azure-cli-latest

Ensuite, connectez-vous 
`# az login`

Il va vous demander d'ouvrir un lien internet avec le meme navigateur ou vous avez votre session ouverte (portal.azure.com). 
`# To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code AC5LZ11DS to authenticate.`

Le CLI va s'authentifier automatiquement et enregistrer en mémoire le token de login pour quelques jours, mais si vous voulez vous déconnecter du CLI vous pouvez toujours faire "az logout"

## Configuration d'Ansible Azure
Pré-requis: la derniere version de Ansible 2.x doit etre installée
Vous pouvez suivre la guide dans http://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html

D'abord on va garder nos 4 ID's dans un fichier de texte, tel que décrit ici http://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html#storing-in-a-file

Par example,  $HOME/.azure/credentials (il faut le protéger avec "chmod 600  $HOME/.azure/credentials")
`# vi $HOME/.azure/credentials
AZURE_CLIENT_ID=11111111-ff8c-4e1e-ab94-517778d11111
AZURE_SECRET=11111111ZjOpJngNV0np0AtWhQXUjawbjt9ca111111=
AZURE_TENANT=11111111-2f1a-492b-bb52-feed52111111
AZURE_SUBSCRIPTION_ID=11111111-cf72-479a-80c8-f22c11111111
`
Pour tester si tout marche bien, on va executer le Dynamic Inventory qui va lister toutes les VMs trouvées dans notre compte. NOTE: c'est possible que vous avez déjà télecharger les fichiers "contrib" de ansible qui ont le script Azure Dynamic Inventory dans votre ordinateur. Cet exemple montre comment le télécharger pour faire ce test
`# curl -o /tmp/azure_rm.py https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
# python /tmp/azure_rm.py`
