Service
=======

**Version: 0.1.0**

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

## Terminology

    MUST - required
    SHOULD - expected but not required

In general the spec avoids 'SHOULD' in favour of 'MUST'. This makes
the spec more onerous, but also more useful.

## Definition

To qualify as a Service requires adhering interfaces at the following
levels of abstraction:

* [aws](/spec/aws.md)
* [containers](/spec/containers.md)
* [http](/spec/http.md)
