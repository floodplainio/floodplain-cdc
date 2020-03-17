# cdc-application project

This is an example application that creates an example change feed.

## Building
The building process is gradle+docker+docker-compose
This should work:
```
./gradlew build
```
to build the source, and then:
```
docker-compose build
```
to build the container.

Then run the containers (both the database and the application):
```
docker-compose up
```


The data model isn't particularly interesting, it is a pretty classical denormalized datamodel
<img src="dvd.svg" width="100%">.

The source for the database container is in this repository:

https://github.com/floodplainio/postgres-debezium-dvdrental

This app simulates an application that makes changes to a database. At the moment of writing, it will insert records into the 'payment' table, referring to random staff, rentals and customer.
Also they will use a random amount.

You can see this happen if you connect to this database. The connection string is:

When connecting from a local machine:

postgresql://postgres:mysecretpassword@localhost:5432/dvdrental

When connecting from a docker container:
(the service name is 'postgres', and this will only work when the container is linked to the network: floodplain-cdc_default)
postgresql://postgres:mysecretpassword@postgres:5432/dvdrental

If you connect using your client of choice, you can check the number of payments:

```sql
select count(*) from payment
```
and the result should steadily increase.

It is written in Quarkus / Kotlin / Java 11
If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```
./gradlew quarkusDev
```

## Packaging and running the application

The application is packageable using `./gradlew quarkusBuild`.
It produces the executable `cdc-application-0.1-runner.jar` file in `build` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `build/lib` directory.

The application is now runnable using `java -jar build/cdc-application-0.1-runner.jar`.

If you want to build an _über-jar_, just add the `--uber-jar` option to the command line:
```
./gradlew quarkusBuild --uber-jar
```
