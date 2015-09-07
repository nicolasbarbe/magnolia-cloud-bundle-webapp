# Magnolia Cloud Bundle Webapp
A Magnolia webapp ready to use for cloud deployment. 

## Features

This bundle implements common practices used in a cloud environment. It provides the following features:
- Configuration through environment variables
- Compliant with [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)
- Single artecfact deployment
- Healthcheck endpoint for easy monitoring (using https://github.com/nicolasbarbe/magnolia-healthcheck)
- Subscriber endpoint for easy author configuration (using https://github.com/nicolasbarbe/magnolia-subscriber-rest-endpoint)

## Usage

Create a Magnolia project using the maven archetype and configure your web application to use this webapp in the `dependencies` section of the maven pom descriptor:
```
    <dependency>
      <groupId>info.magnolia.cloud.bundle</groupId>
      <artifactId>magnolia-cloud-bundle-webapp</artifactId>
      <version>${magnoliaVersion}</version>
      <type>war</type>
    </dependency>

    <dependency>
      <groupId>info.magnolia.cloud.bundle</groupId>
      <artifactId>magnolia-cloud-bundle-webapp</artifactId>
      <version>${magnoliaVersion}</version>
      <type>pom</type>
    </dependency>
```

Use the following environment variables to configure the artefact during its deployment:

- `INSTANCE_TYPE` : Type of the instance, can be either `author` or `public`. Default is `author`.
- `DB_TYPE` : Type of the instance, can be either `derby`, `mysql` or `postgresql`. Default is `derby`.

Magnolia will automatically loads the corresponding `magnolia.properties`. Note that `derby` is not suitable for production usage to store page content.

The database credentials can be set trough the Java properties:

- `db.address` : IP address of the database server.
- `db.port` : Port of the database server.
- `db.username` : Name of the user to connect to the database server.
- `db.password` : Password of the user to connect to the database server.
- `db.schema` : Database schema (usefull if the database server is shared accross multiple magnolia instances).

The environment variable `CATALINA_OPTS` can be used to set those variables:
```
CATALINA_OPTS="$CATALINA_OPTS \
  -Ddb.address=10.0.0.1 \
  -Ddb.port=3306 \
  -Ddb.schema=magnolia \
  -Ddb.username=magnolia \
  -Ddb.password=secret" 
```

The default `magnolia.properties` follows the recommendations of the [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard):

|property  | location |
| ------------- | ------------- |
| magnolia.resources.dir  | /var/opt/magnolia  |
| magnolia.cache.startdir | /var/cache/magnolia |
| magnolia.upload.tmpdir | /var/tmp/magnolia/uploaded |
| magnolia.exchange.history | /var/tmp/magnolia/history |
| magnolia.repositories.home | /var/lib/magnolia/repositories |
| magnolia.logs.dir | /var/log/magnolia |
| magnolia.author.key.location | /var/lib/magnolia/magnolia-activation-keypair.properties |

