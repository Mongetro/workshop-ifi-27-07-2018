# Preparation pour contribuer à Apache JAMES

1. Récupérez les sources:

 - Forkez https://github.com/apache/james-project (boutton en haut à droite)
 - `git clone https://github.com/XXX/james-project` vous permettra d'avoir les sources disponibles su votre ordinateur.

2. Mise en place du système de build:

 - Téléchargez installez la dernière version de [maven](http://maven.apache.org)
 - Tapez `mvn clean install -DskipTests` pour compiler James (et ainsi télécharger les dépendances)
 
3. Mise en place de l'IDE

Note: Je vais détailler mon setUp, mais vous pouvez parfaitement importer le projet James dans votre IDE préféré.

 - Téléchargez et installez [IntelliJ](https://www.jetbrains.com/idea/download/#section=linux)
 - Créer un nouveau project (from existing sources) en temps que projet maven. Et laissez IntelliJ mouliner une bonne demie-heure.
 
4. Essayez Apache James

Vous pouvez simplement éxécuter Apache JAMES en utilisant cette commande:

```
docker run -p "25:25" -p "143:143" linagora/james-jpa-sample:3.0.1
```

Vous pouvez ensuite configurer ThunderBird sur votre ordinateur pour les utilisateurs suivants: user01@james.local, user02@james.local, user03@james.local (mot de passe: 1234)
