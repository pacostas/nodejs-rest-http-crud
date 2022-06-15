# Example CRUD App with OpenTelemetry and Jaeger

## Getting Started

### Running Locally

First, install the dependencies

`npm install`

A Postgres DB is needed, so if you are using Docker, then you can start a postgres db easily.

`docker run --name os-postgres-db -e POSTGRESQL_USER=luke -e POSTGRESQL_PASSWORD=secret -e POSTGRESQL_DATABASE=my_data -d -p 5432:5432 centos/postgresql-10-centos7`

In this example, the db user is `luke`, the password is `secret` and the database is `my_data`

You can then start the application like this:

`DB_USERNAME=luke DB_PASSWORD=secret ./bin/www`

Then go to http://localhost:8080

### Running on OpenShift

First, make sure you have an instance of OpenShift setup and are logged in using `oc login`.

Then create a new project using the `oc` commands

`oc new-project opentel`

For this example, you will also need a postgres db running on your OpenShift cluster.

`oc new-app -e POSTGRESQL_USER=luke -ePOSTGRESQL_PASSWORD=secret -ePOSTGRESQL_DATABASE=my_data centos/postgresql-10-centos7 --name=my-database`

Then run `npm run openshift` to deploy your app.

#### Installing operators

Log in with kubeadmin and install the collector and jaeger via operator hub.

Give to `developer` user admin rights on the project

```
oc policy add-role-to-user admin developer -n opentel
```

Create a Jaeger instance

```
oc apply -f extra/jaeger.yml
```

Create an OpenTelemetry instance

```
oc apply -f extra/opentel-collector.yml
```