# Desarrollo AWS local 
Un reto común en el desarrollo de aplicaciones en la nube, o de la modernización de aplicaciones en cualquiera de sus diferentes estrategias es las limitaciones en la generación de entornos de desarrollo locales que permitan al equipo de desarrollo con herramientas de acceso rápido con mínimo impacto en costo y con total control para pruebas unitarias.
La creación de arquetipos de trabajo que permitan el desarrollo de estas estrategias es el objeto de este artefacto. Recorriendo las diferenntes opciones disponibles para lograrlo.
Se platea la siguiente hipótesis:  Es posible desarrollar localmente aplicaciones de nativas de nube utilizando recursos locales utilizando arquetipos de trabajo que minimicen el costo y permitan realizar pruebas unitaria. 

## Objetivos
Se plantean los siguientes objetivos:
* Definición de arquetipos de trabajo para desarrollo de aplicaciones en la nube
* Definición de recursos locales que apoyen el desarrollo de aplicaciones basado en estos arquetipos.

## Requerimientos y Premisas
Para el dearrollo de esta investigación nos enfocaremos en el l a creación de un ambiente (stack) de dearrollo local que sirva de marco de trabajo para AWS como proveedor de nube.
Otros proveedores podrán irse agregando posteriormente según la investigación logre sus objetivos o sume colaboradores.

Para la prueba del trabajo propuesto se requiere:
* Docker
* awscli
* git
* nodejs

## Definicion de recursos locales 
Para el desarrollo y pruebas del desarrollo es importante contar con ambientes locales que permitan la validación rápida del trabajo. 
Cuando se trata de dearrollo en la nube se plantea un reto a las pruebas locales, si bien se gana en flexibilidad para la gestión de los recursos globales, cubrir la curva 
de apredizaje  de desarrolladores puede impactar el costo  o dificultar su control. Es por ello que crear un infraestrutura que modele los servicios de nube pero en 
ambientes locales resulta una gran ventaja al desarrollo que se  libera incluso de la necesidad de conexión para avanzar y probar el desarrollo.

En la investigación realizada, se encontró que una excelente alternativa esta representada por localstack. 
LocalStack proporciona un marco de trabajo / prueba fácil de usar para desarrollar aplicaciones en la nube.

### Trabajando con LocalStack
LocalStack es una herramienta muy util para la creacion de un ambiente local de desarrollo y prueba. 
```bash
docker run --name localstacksvr \
  --rm -d  -it -p 4566:4566 -p 4571:4571 \ 
  localstack/localstack -e "SERVICES=dynamodb,s3" 
```
Para probar el servicio de dynamoDB se puede utilizar la herramienta de administración dynamodb-admin
Para instalarla ejecutamos el comando
```bash
npm install -g dynamodb-admin
```
Para ejecutarla en Windows:
```cmd
set DYNAMO_ENDPOINT=http://localhost:4566
dynamodb-admin
bash
```

Para ejecutarla en Mac/Linux:
```bash
DYNAMO_ENDPOINT=http://localhost:4566 dynamodb-admin
```

Para crear la infraestrutura utilizamos el template de cloudformation cf-template.yaml

```bash
aws --endpoint-url http://localhost:4566 \
  cloudformation create-stack \
  --stack-name samplestack \
  --template-body file://cf-template.yaml \
  --profile localstack
```


## Referencias
* [Use Amazon DynamoDB local](https://aws.amazon.com/es/about-aws/whats-new/2018/08/use-amazon-dynamodb-local-more-easily-with-the-new-docker-image/)
* [Local Development with AWS on LocalStack](https://reflectoring.io/aws-localstack/)
* [Project dynamodb-admin](https://www.npmjs.com/package/dynamodb-admin)
* [Commandeer](https://getcommandeer.com/)
* [Google Functions Framework for Node.js](https://github.com/GoogleCloudPlatform/functions-framework-nodejs)
* [Functions Framework for Java](https://github.com/GoogleCloudPlatform/functions-framework-java)
* [Subneting IP Calculator](http://jodies.de/)