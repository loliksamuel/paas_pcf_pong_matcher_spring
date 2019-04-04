# PCF - Pivotal Cloud Foundry paas installation
* brew tap cloudfoundry/tap
* brew install <cf-cli | bosh-init | bosh-cli | credhub-cli | bbl | bbr>
* https://github.com/cloudfoundry-samples

# PCF learning

This is an app to match ping-pong players with each other. It's currently an
API only, so you have to use `curl` to interact with it.

It has an [acceptance test suite][acceptance-test] you might like to look at.

**Note**: We highly recommend that you use the latest versions of any software required by this sample application. For example, make sure that you are using the most recent verion of maven.

## Building(create jar file)**
```
$ git clone [REPO]
$ cd [REPO]
$ ./mvnw clean install
``` 

## Running on [Pivotal Web Services][pws]

Log in.

```bash
cf login -a https://api.run.pivotal.io
```

Target your org / space.

```bash
cf create-space spc1 -o allocation

cf target -o allocation -s spc1
```

Sign up for a cleardb instance.

```bash
cf create-service cleardb spark mysql
```

Push the app. Its manifest assumes you called your ClearDB instance 'mysql'.

```bash
cf push pongdemo -p target/springpong-0.0.1-SNAPSHOT.jar -n mysubdomain4
```

Export the test host
wait and see long logs
see the route
http://mysubdomain4.cfapps.io/all
```bash
export HOST=http://mysubdomain4.cfapps.io
then
pcf stop springpong
```

Now follow the [interaction instructions][interaction].
https://run.pivotal.io/

## Running locally

The following assumes you have a working Java 1.8 SDK installed.

Install and start mysql:

```bash
brew install mysql
mysql.server start
mysql -u root
```

Create a database user and table in the MySQL REPL you just opened:

```sql
CREATE USER 'springpong'@'localhost' IDENTIFIED BY 'springpong';
CREATE DATABASE pong_matcher_spring_development;
GRANT ALL ON pong_matcher_spring_development.* TO 'springpong'@'localhost';
exit
```

Start the application server from your IDE or the command line:

```bash
mvn spring-boot:run
```

Export the test host

```bash
export HOST=http://localhost:8080
```

Now follow the [interaction instructions][interaction].

[acceptance-test]:https://github.com/cloudfoundry-samples/pong_matcher_acceptance
[pws]:https://run.pivotal.io
[interaction]:https://github.com/cloudfoundry-samples/pong_matcher_rails/blob/master/README.md#interaction-instructions


