HTTP
====

## Resources

Each EC2 instance MUST present the following (GET) resources:

    [/service/healthcheck](#healthcheck)
    [/service/gtg](#gtg)
    [/service/dependencies](#dependencies)

These resources should be presented on port 8080 (not port 80).

### <a name="gtg">GTG</a>

'Good to go' - is the service running? Two possible responses:

    200 (service is running)
    50* (service is down - choose exact status code as appropriate)

Body is any valid Argo response or error response as appropriate.

### <a name="healthcheck">Healthcheck</a>

    200 (service is able to usefully respond to requests)
    50* (service is down - choose exact status code as appropriate)

Is the service able to usefully respond to requests? (Typically, this
indicates whether the service is running and its dependencies are
available as expected.)

This endpoint should list dependencies along with their (perceived)
health.

    {
        data: [
            {
                name: "some-dep",
                status: "okay", // or "error"
                testedAt: "2015-04-01T12:33:01.343Z"
            },
            ..
        ]
    }

(The endpoint MUST be a valid Argo response - other fields can be
added as desired, but the data property must be returned in this
format.)

Note, *this endpoint MUST not block*. It should return the most recently
executed dependency checks.

## Other

For applications, such as APIs, which allow interaction via HTTP(S),
the following additional requirements apply:

* HTTPS MUST be supported.
* Appropriate HTTP response codes MUST be used.
* GZIP MUST be supported.
* JSON SHOULD be used as the transport format. In cases where a binary
  protocol is found to be required, Thrift is recommended. Note, JSON
  MUST always be used for the special service endpoints.
* A version number MUST be included in the URL (e.g. '/v1').
* A 'fields' parameter SHOULD be supported to minimise bandwidth.
* CORS SHOULD be supported.
