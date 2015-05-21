HTTP
====

## Resources

Each EC2 instance MUST present the following (GET) resources:

    [/service/healthcheck](#healthcheck)
    [/service/gtg](#gtg)
    [/service/dependencies](#dependencies)

These resources should be presented on port 8080 (not port 80).

### <a name="healthcheck">Healthcheck</a>

Is the service running? Two possible responses:

    200 "OK" (service is running)
    50* (service is down - choose exact status code as appropriate)

### <a name="gtg">GTG</a>

    200 "OK" (service is able to usefully respond to requests)
    50* (service is down - choose exact status code as appropriate)

Is the service able to usefully respond to requests? (Typically, this
indicates whether the service is running and its dependencies are
available as expected.)

### <a name="dependencies">Dependencies</a>

Provides information on service dependencies including current
availability. (Roughly equivalent to SE4 /healthcheck endpoint.)

See the
[SE4 healthcheck endpoint](https://github.com/beamly/SE4/blob/1.0/SE4.md#healthcheck)
definition here.

## Other

For applications, such as APIs, which allow interaction via HTTP(S),
the following additional requirements apply:

* HTTPS MUST be supported.
* Appropriate HTTP response codes MUST be used.
* GZIP MUST be supported.
* JSON MUST be used as the transport format. In cases where a binary
  protocol is found to be required, Thrift is recommended.
* A version number MUST be included in the URL (e.g. '/v1').
* A 'fields' parameter SHOULD be supported to minimise bandwidth.
* CORS SHOULD be supported.
