# Build instructions

## Building the Java service

`mvn clean install` should be enough. This will build the sources and run the unittests.

## Building the docker images and using them for local testing

The easiest way to build the docker images is to use the script `build-dockers.py`.

Run it with `--help` to see what options it supports. As a developer, you should normally
not need to use any options to build a set of images suitable for running local development
and tests. However, the `--pull` option may be convenient to ensure that base 
images are pulled to the newest. And the `--use-cache` option can be used in some cases to 
reduce build times. Use at your discretion.

Build the images e.g like this:

```bash
mvn install
...
build-dockers.py
```

This will create the images with a prefix of `culr-local` and a tag of `latest`,
enabling you to run a simple test of the containers like this

```bash
cd docker/compose/dev && \
docker-compose up -d && \
cd -
```

When the containers are ready, you can run a simple SOAP request against the CulrService container like this:

```bash
curl -vf  -d @culrservice-ws/src/test/request/request1.xml  \
  -H "Accept:text/xml" -H "Content-Type:text/xml" \
  http://localhost:18380/1.4/CulrWebService ; echo
```

If the service is not (yet) ready, you will get a 404.

If you get a SOAP answer, the service is running in the container. It will most likely be an answer complaining about
missing auth information, as the password in the request is not correct:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
  <S:Body>
    <ns2:deleteAccountResponse xmlns:ns2="http://ws.culrservice.dbc.dk/">
      <return>
        <responseStatus>
          <responseCode>NO_AUTHORISATION</responseCode>
          <responseMessage>The user credentials provided could not be authenticated</responseMessage>
        </responseStatus>
      </return>
    </ns2:deleteAccountResponse>
  </S:Body>
</S:Envelope>
```

## Systemtest and performancetest 

The systemtest tests the internals of culr, inclusive the SFTP transfer of 
CPR numbers. The performancetest is meant to test e.g. memory usage with many
registrations, etc. Note, that these tests can be run in parallel, as they use 
different docker-compose project names, networks, etc.

### Running the systemtest

To run the systemtest locally, build the docker images, then

```bash
./run-system-test.sh 
```

The script supports a number of options, use `--help` to get more info. 

Example: For debugging purposes, you can the system test, and keep the containers running afterwards, 
using the `--keep`argument. If you also wish to make certain that the non-project containers
used in the test is pulled from the repo, use the `--pull` option:

```bash
./run-system-test.sh --keep --pull
```

The systemtest will produce a lot of output. Look for a last line like this:

`[systest] 18:46:16.744352360 INFO: Test PASSED`

This means the test was a success.

### Running the performancetest

You can run performance tests very much like the systemtest. Follow the approach for building, etc, for 
the systemtest, but use e.g. a statement like this, for running the performance test
```bash
./run-performance-test.sh 
```

Note though, that you can provide an extra argument `--short` to run the performancetest with only 1000 iterations, 
instead of the normal 7000000. Useful during development of the
performancetest itself.
