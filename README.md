This is NOT an official version of Liquibase Docker! The official location is https://github.com/liquibase/docker. The official liquibase-docker container is located on DockerHub at https://hub.docker.com/r/liquibase/liquibase.

If you are not Erzsebet Carmean or one of her esteemed collegues, you will be happier, fitter, smarter and more gorgeous if you use the official versions!  

#####################################################
#### RUNNING LIQUIBASE DOCKER and DOCKER POSTGRES ###
#####################################################

**** Create Network ****

docker network create liquibase --attachable --subnet 10.0.0.0/24

**** Start Postgres Container ****

docker run --network liquibase --ip="10.0.0.3" --name=pg_12 --hostname=pg_12_host -p5433:5432 -e POSTGRES_PASSWORD=password -d postgres:12.6 

**** Attach Postgres to Network ****

docker network connect liquibase pg_12

**** Configure Properties File ****

NOTE: Liquibase Docker connects to 5432 when on the same network as Postgres.

url: jdbc:postgresql://10.0.0.3:5432/goku

username: lbuser1

password: password

changelogFile: postgres_lbpro_master_changelog.xml

liquibase.hub.apiKey: <YOUR KEY>

liquibaseProLicenseKey: PRO

classpath: /liquibase/changelog/:/liquibase/changelog/postgresql-42.2.18.jar

**** Start Liquibase and Connect to Network ****

docker run --rm --network liquibase --ip=10.0.0.4 -v C:\dev\DaticalDB-testing\liquibase-pro-cli-project\postgres_lbpro_master:/liquibase/changelog daticalerzsebet/docker --defaultsFile=/liquibase/changelog/liquibase.docker.properties --logLevel=FINE update

**** Run Liquibase with an Interactive Terminal to Register a Changelog ****
  
docker run -it  --rm --network liquibase --ip=10.0.0.4 -v C:\dev\DaticalDB-testing\liquibase-pro-cli-project\postgres_lbpro_master:/liquibase/changelog daticalerzsebet/docker --defaultsFile=/liquibase/changelog/liquibase.docker.properties --logLevel=FINE --logFile=/liquibase/changelog/24V21.txt registerChangelog
