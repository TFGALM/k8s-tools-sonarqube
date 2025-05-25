# sonarqube

``` shell
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
helm repo update
## https://artifacthub.io/packages/helm/sonarqube/sonarqube/10.5.1+2816
## helm show values sonarqube/sonarqube --version 10.5.1+2816 > values.default.yaml
kubectl create namespace sonarqube
kubectl apply -f manifests
helm upgrade --install -f values.yaml sonarqube sonarqube/sonarqube -n sonarqube --create-namespace --version 10.4.1+2816 
```
**Note:** 10.5.1+2816 version is not compatible with https://github.com/mc1arke/sonarqube-community-branch-plugin/releases/tag/1.19.0 we must use 10.4.1 and values is not the same.

# Plugin installation

1. Download the plugin you want to install. The version needs to be compatible with your SonarQube version.
2. Put the downloaded jar in <sonarqubeHome>/extensions/plugins, and remove any previous versions of the same plugins.
```bash
    kubectl cp <PATH_TO_PLUGIN_FILE> <POD_NAME>:<sonarqubeHome>/extensions/plugins> -c sonarqube -n sonarqube
```
Example:
```bash
    kubectl cp  sonarqube-sonarqube-0:/opt/sonarqube/extensions/plugins -c sonarqube -n sonarqube
```
    
3. Restart your SonarQube server.

## Necessary plugins
- https://github.com/mc1arke/sonarqube-community-branch-plugin/releases/tag/1.19.0

## TODO:

- Desarrollar un fichero helmfiles para que sea parametrizable con variables globales y hacerlo replicable entre clusteres.
- La bdd que monta este helm solo es para evaluacion,  https://artifacthub.io/packages/helm/sonarqube/sonarqube#production-use-case
    - Su recomendacion es que te instancies tu propio postgresQL y que tengas tu propio elasticSearch también
- Todo, configurar el usuario admin por defecto y el rol admin a partir del grupo de keycloak
