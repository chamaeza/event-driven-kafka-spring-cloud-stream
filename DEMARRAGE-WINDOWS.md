# Projet Kafka / Spring Cloud Stream - Windows / IntelliJ

## Adaptation

Ce ZIP reprend le projet fourni (PageEvent, publication REST, consumer, supplier, Kafka Streams, analytics SSE et page de visualisation).
Adaptation pour **Java 21**, **Spring Boot 3.5.16**, **Spring Cloud 2025.0.0** et **PowerShell Windows**.
Port de cette application : **8090** (pour eviter le port 8083 utilise par le projet Angular et Billing Service).
Kafka utilise le port **9092**. Pas besoin de Node.js ni Angular pour ce projet.

## Prerequis

- Java 21 : `C:\Users\ezzch\.jdks\ms-21.0.10` (verifier l'existence sur votre PC)
- Docker Desktop installe et lance, avec Docker Compose disponible (`docker compose version`)
- Connexion internet au premier lancement pour les images Docker et les dependances Maven
- IntelliJ IDEA : File > Open > dossier de ce projet (avec pom.xml)

## Etape 1 - Ouvrir PowerShell dans ce dossier

```powershell
$env:JAVA_HOME = 'C:\Users\ezzch\.jdks\ms-21.0.10'
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
java -version
```

## Etape 2 - Demarrer Kafka dans Docker

```powershell
docker compose up -d
docker compose ps
```

Attendre que Kafka soit pret. Pour les logs : `docker compose logs -f kafka` (Ctrl+C quitte les logs sans arreter Kafka).

## Etape 3 - Demarrer Spring Boot (dans un AUTRE terminal)

Configurer JAVA_HOME dans ce nouveau terminal egalement.

```powershell
$env:JAVA_HOME = 'C:\Users\ezzch\.jdks\ms-21.0.10'
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
.\mvnw.cmd spring-boot:run
```

Si le Maven Wrapper ne fonctionne pas, essayer le wrapper deja utilise sur votre PC :

```powershell
& 'C:\Users\ezzch\IdeaProjects\customer-service\mvnw.cmd' spring-boot:run
```

## Etape 4 - Tester

- Interface graphique : http://localhost:8090/
- Publier un evenement vers T1 : http://localhost:8090/publish?name=P1&topic=T1
- Publier un evenement vers T6 (alimente les analytics) : http://localhost:8090/publish?name=P1&topic=T6
- Flux SSE analytics : http://localhost:8090/analytics

Le supplier genere aussi des evenements dans T6 toutes les secondes.

## Arreter

Dans le terminal Spring Boot : Ctrl+C.
Dans le terminal Docker : `docker compose down`.

## Notes

- Ne pas lancer deux applications sur le meme port.
- Ce projet n'a pas besoin des 6 microservices precedents pour fonctionner.
- Le projet source comportait un fichier commands.txt avec des commandes Kafka anciennes : privilegier Docker Compose ici.
- Les versions des dependances n'ont pas ete compilees sur votre PC dans cette archive. Si Maven signale une incompatibilite, envoyer le log exact.
