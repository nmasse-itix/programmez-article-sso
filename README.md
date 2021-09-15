# Code de l'article "Red Hat SSO" pour le magazine Programmez !

## Téléchargement

```sh
git clone https://github.com/nmasse-itix/programmez-article-sso.git
cd programmez-article-sso
./mvnw compile quarkus:dev
```

## Requêtes d'exemple

```sh
http http://localhost:8080/pets/
http http://localhost:8080/pets/1
http http://localhost:8080/pets/2
http POST http://localhost:8080/pets name=Medor tag=dog
http PUT http://localhost:8080/pets/3 name=Brutus tag=dog
http DELETE http://localhost:8080/pets/3
```

## Obtenir un jeton OpenID Connect

```sh
function get_token () { 
  curl -sk -XPOST -d grant_type=password -d scope=openid \
  https://$SSO_HOSTNAME/auth/realms/$SSO_REALM/protocol/openid-connect/token \
  -d client_id=$CLIENT_ID -d client_secret=$CLIENT_SECRET \
  --data-urlencode username=$LOGIN \
  --data-urlencode password=$PASSWORD \
  | jq -r 'if .access_token then .access_token else . end'
}

SSO_HOSTNAME=keycloak-developpez.apps.itix-dev.ocp.itix
SSO_REALM=developpez
CLIENT_ID=client-rest
CLIENT_SECRET=375b9c88-3f42-44c5-8b65-ac501fd8db8f
LOGIN=john
PASSWORD=s3cr3t

get_token
```

## Passer le jeton OpenID Connect en paramètre

```sh
http http://localhost:8080/pets/ "Authorization:Bearer $(get_token)"
```
