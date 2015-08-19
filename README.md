person example
---------

Example how to access a DataSource using OSGi services and jdbc and retur result
to Json REST format using cxf.


Installation
------------
karaf@root()> feature:repo-add camel 2.15.2
karaf@root()> feature:repo-add cxf 2.7.11
karaf@root()> feature:install cxf/2.7.11
karaf@root()> feature:install camel
karaf@root()> feature:install camel-cxf
karaf@root()> feature:install camel-core
karaf@root()> feature:install camel-spring
karaf@root()> feature:install camel-jpa
karaf@root()> feature:install camel-jackson
karaf@root()> feature:install jdbc


feature:install transaction jndi pax-jdbc-h2 pax-jdbc-config pax-jdbc-pool-dbcp2 jpa/2.1.0 hibernate/4.3.6.Final

place the datasource-postgres.xml in the deploy folder.

Build
-----

mvn clean install

copy person-module-1.0-SNAPSHOT.kar in the person-module and place it in the deploy folder
this install features : person-module and person-dependants
and tow bundles : person-template and person-persistencece

Test
----

Step 0 : verify if the bundles are installed and active :
karaf@root()> list

Step 1 : verify if the connexion with postgres database work fine 
two Karaf shell commands:

karaf@root(person)> person:add a @a
karaf@root(person)> person:list
a, @a
-> Lists all persisted Persons


Step 2 : verify if the web service is deployed : 
cxf:list-endpoints
Name                      State      Address                                                      BusID                                   
 RestServiceImpl     Started     http://localhost:8181/cxf/person   person-template-cxf951148374 


use POSTMAN plugin to test web service :
URL : http://localhost:8181/cxf/person/rest/message
Method => POST
param (body) => message = x

result => 
{
  "jsonResponseResult": {
    "message": "Hello x"
  }
}
