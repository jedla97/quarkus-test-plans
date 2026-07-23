# QUARKUS-7866 - Add JsonProvider SPI for per-record dynamic JSON field additions

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7866

Upstream PR: https://github.com/quarkusio/quarkus/pull/53415

This feature introduces a new `JsonProvider` SPI that allows applications to dynamically add custom fields to JSON log records.

## Scope of the testing
Verify that `JsonProvider` implementations correctly add dynamic JSON fields across all supported JSON logging handlers.

For each JSON logging handler: console, file, syslog and socket verify the following:

* **Fields added to every log record:** Verify that custom fields added by the `JsonProvider` are present in the emitted JSON.
* **Fields based on log content:** Verify that the provider can read information from the current log record and include it in an additional JSON field.
* **Fields added only in specific cases:** Verify that a field is present only in ERROR records and absent from INFO records
* **Fields based on request context:** Verify that a MDC value such as `requestId` is included when present and omitted when unavailable.
* **Nested JSON fields:** Verify that providers can generate nested JSON structures.
* **Excluded JSON fields:** Verify that provider generated fields configured in `excluded-keys` are filtered from the final JSON output.

For the **console handler** only, verify both CDI registration and ServiceLoader registration. 

## Existing test coverage

Upstream tests the `JsonProvider` SPI by invoking the JSON formatter directly. The existing tests verify custom field generation, conditional field generation, MDC integration, nested JSON objects, CDI and Java ServiceLoader registeration and excluded-keys filtering.

The QE coverage will test the feature from a user perspective. The tests will start a Quarkus application, trigger logging through a REST endpoint and verify that provider generated fields are present in the actual JSON produced by the logging handlers.

### Impact on test suites and testing automation

* Console and File tests will be added under the `logging/jboss` module.
* Syslog and Socket tests will be added under the `logging/thirdparty` module.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal in JVM and native mode.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream issue and pull request.
* Reviewed documentation on custom JSON fields with JsonProvider.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/logging#json-provider