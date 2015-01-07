Service
=======

Defines a Service abstraction for apps in the cloud.

## Purpose

When running several small services, as opposed to a few very big
ones, it is useful to standardise certain elements.

Benefits include: improved developer understanding (born out of
familiarity), and the emergence of generic tools - for things such as
monitoring and alerting - which can improve quality and reduce
duplication of effort across projects.

To clarify what needs standardising, we define a 'Service'
abstraction, in effect a contract which services should meet to be
production ready.

Service is initially a product of the Guardian Discussion team.

## Definition

To qualify as a Service requires:

* following certain AWS conventions
* one-click deploys and stack creation
* some specific documentation
* sufficient monitoring and alerting

Harder to quantify, but equally important, if not more so, is:

* a decoupled architecture, including no hard-dependencies (graceful
  failure)

## Spec

    MUST - required
    SHOULD - expected but not required

### Naming conventions

The general pattern is:

    [stack]-[app]-[STAGE]

(Clauses separated by hyphens, and the stage capitalised.)

In certain cases, further information may be appended onto the end of
this pattern as appropriate. For example, a Cloudwatch alarm might be
called `discussion-search-PROD-disk-util`.

#### S3

S3 names are global. As a result, they should be prefaced with
`com-gu-discussion`.

Note, '-' is preferred to '.' as a delimiter, because of SSL issues
(see:
https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/).

### Tags

Each EC2 instance MUST have the following tags:

* Stack
* Stage (value is capitalised)
* Name

### Resources

Each EC2 instance MUST present the following (GET) resources:

    [/service/healthcheck](#healthcheck)
    [/service/gtg](#gtg)
    [/service/dependencies](#dependencies)

These resources should be presented on port 8080 (not port 80).

#### <a name="healthcheck">Healthcheck</a>

Is the service running? Two possible responses:

    200 "OK" (service is running)
    50* (service is down - choose exact status code as appropriate)

#### <a name="gtg">GTG</a>

    200 "OK" (service is able to usefully respond to requests)
    50* (service is down - choose exact status code as appropriate)

Is the service able to usefully respond to requests? (Typically, this
indicates whether the service is running and its dependencies are
available as expected.)

#### <a name="dependencies">Dependencies</a>

Provides information on service dependencies including current
availability. (Roughly equivalent to SE4 /healthcheck endpoint.)

See the
[SE4 healthcheck endpoint](https://github.com/beamly/SE4/blob/1.0/SE4.md#healthcheck)
definition here.

### Monitoring and alerting

The following metrics MUST be collected for all instances:

* CPU utilisation (%)
* Memory usage (%)
* Disk utilisation (%)

### Timeouts, limits, and dependencies

Requests to a service SHOULD have explicit timeouts configured.

Requests from a service to dependencies SHOULD have explicit timeouts
configured and be wrapped in a Circuit Breaker.

### Cloudformation and deploys

AMIs MUST be built for each deploy, with any dependencies built
in. This ensures startup (and so recovery times!) are quick.
