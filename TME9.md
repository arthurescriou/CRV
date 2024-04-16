# TME 9

L'objectif de ce TME est de lancer des tests de charge et d'observer le comportement de notre serveur.

## Le serveur

Le serveur que nous voulons tester est celui du projet 1.

C'est un serveur node.js utilisant le framework express.js.

D'après les recommandations et les spécifications donné des équipes de ces technologies : node.js ne peut pas fonctionner avec moins de 512 mb de RAM et express.js doit avoir au moins 1 gb de RAM pour fonctionner correctement.

## Le script de test

Pour tester le comportement du serveur nous avons des scripts permettant d'appeler le serveur avec différents comportements :

- beaucoup d'appel sur une route ne lançant pas de calcul important
- des appels variés sur des routes appelant des intéractions avec la base de données
- des appels repondant très lentement (pouvant déclencher des timeout)

En croisant ces scripts avec plusieurs appels à la fois on veut chercher des comportements limites du serveur.

### Vérifier que le script appel le serveur

Le script appel un URL (par défaut `http://localhost:8080`).

Il est possible de modifier le fichier `fetchData.js` ou d'injecter la valeur voulue dans la variable d'environment URL.

Lancez le script pour tester la connexion.

```bash
node fetchData.js
```

### Lancez un scénario de test basique

On veut maintenant tester que plusieurs appels en parallèle fonctionne

```bash
node fetchData.js server 1000 100
```

### Lancez plusieurs test en même temps

Pour tester le comportement du serveur on va lancer plusieurs script en même temps.

On veut ouvrir et laisser en pending des connections pendant qu'on lance d'autres tests

```bash
node fetchData.js pending 200 10000
```

Pendant que ce script s'exécute lancer un autre test de charge.

```bash
node fetchData.js writeRead 10000 100
```

### Monter la charge jusqu'au crash du serveur

Pour vérifier que notre test de charge utilise suffisamment de ressources lancez un script suffisamment lourd pour que le serveur crash et redémarre.

Il est possible d'observer le crash via les logs du serveur ou des messages d'erreurs du script.

### Déclencher une montée d'échelle

Pour vérifier que l'autoscaling du projet 1 fonctionne correctement, configurez un auto-scaling de 1 à 3 pods et limitez les ressources du pods à 1 gb de RAM et 1 cpu.

Lancez un script pour déclencher une montée à l'échelle du déploiement.
