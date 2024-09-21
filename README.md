# cmd-k8s

La commande que tu as exécutée, kubectl apply -k https://github.com/argoproj/argo-cd/manifests/crds?ref=stable, a installé les Custom Resource Definitions (CRDs) pour Argo CD dans ton cluster Kubernetes. Les CRDs permettent à Kubernetes de reconnaître de nouvelles ressources personnalisées propres à Argo CD.
Explication des CRDs créées :

    applications.argoproj.io :
        Cette CRD représente une application Argo CD, c’est-à-dire un déploiement Kubernetes géré par Argo CD. Une ressource Application peut définir où se trouve le code (repository Git), comment le déployer et quel cluster cible.

    applicationsets.argoproj.io :
        Cette CRD est pour les ApplicationSets. Elle permet de gérer plusieurs applications à partir d'une seule configuration. Elle est souvent utilisée dans les déploiements multi-environnements ou multi-clusters.

    appprojects.argoproj.io :
        Cette CRD est pour les AppProjects. Elle permet de regrouper plusieurs applications sous un même projet. Un AppProject est souvent utilisé pour définir des politiques d'accès et des autorisations sur plusieurs applications au sein d'un projet.

Que faire ensuite ?

    Vérifier les CRDs installées : Tu peux vérifier que les CRDs sont bien enregistrées dans ton cluster avec la commande suivante :

    bash

kubectl get crds

Cela devrait te montrer les trois CRDs installées (applications.argoproj.io, applicationsets.argoproj.io, et appprojects.argoproj.io).

Installer Argo CD : Maintenant que les CRDs sont en place, tu peux procéder à l'installation d'Argo CD en appliquant les manifestes d'installation.

Utilise cette commande pour déployer Argo CD dans ton cluster :

bash

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Cela créera les composants nécessaires à Argo CD comme les pods et services pour le serveur API, le contrôleur, et l'interface utilisateur.

Vérifier les ressources Argo CD : Une fois installé, tu peux vérifier les pods Argo CD dans le namespace argocd :

bash

kubectl get pods -n argocd

Accéder à l'interface utilisateur d'Argo CD : Tu peux accéder à l'interface web d'Argo CD en exposant le service via NodePort ou LoadBalancer ou en utilisant un port-forward :

bash

kubectl port-forward svc/argocd-server -n argocd 8080:443

Ensuite, tu pourras accéder à l'interface via : https://localhost:8080.

Récupérer le mot de passe : Le mot de passe par défaut pour l'utilisateur admin est stocké dans un secret. Pour le récupérer, exécute :

bash

    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d

Ensuite, tu pourras te connecter à l'interface utilisateur Argo CD avec admin comme nom d'utilisateur et le mot de passe récupéré.
