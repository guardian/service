AWS
===

## Naming and metadata conventions

Components of names are:

    org   -> the organisation producing the service, e.g. com.gu
    stack -> conceptual group the app belongs too
    app   -> the name of the service itself
    STAGE -> the environment the app is running in (e.g. PROD), must
             be capitalised

The general pattern is:

    [stack]-[app]-[STAGE]

(Clauses separated by hyphens, and the stage capitalised.)

In certain cases, further information may be appended onto the end of
this pattern as appropriate. For example, a Cloudwatch alarm might be
called `discussion-search-PROD-disk-util`.

### S3

S3 names are global. As a result, they should be prefaced with
`[org]-[stack]`. For the Guardian, this might look like:
`com-gu-discussion`.

Note, '-' is preferred to '.' as a delimiter for bucket names, because
of SSL issues (see:
https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/).

### Tags

Every single AWS resource MUST have the following tags:

* Stack
* Stage

Depending on the resource, additional tags should also be specified:

* App

Note, where the resource is used across stages, use 'INFRA' as the
stage.

## Monitoring and alerting

The following metrics MUST be collected for all instances:

* CPU utilisation (%)
* Memory usage (%)
* Disk utilisation (%)

Metrics MUST be stored in a central location. Typically, this SHOULD
mean Cloudwatch.

## Provisioning

All resources MUST be generated via Cloudformation.
