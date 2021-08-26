# QUARKUS-1079 - Hibernate Reactive as Tech Preview

JIRA link: https://issues.redhat.com/browse/QUARKUS-1079

For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing
This feature added previews of two extensions:
 - Hibernate Reactive
 - Hibernate Reactive with Panache

Following test coverage should be implemented for Hibernate Reactive:
- Usage of application.properties file for passing reactive-hibernate settings
- Usage of import.sql
- Smoke test(getting a working Mutiny session)
- Retrieving Multi result from the DB
- Retrieving Uni result(require a transformation) from the DB
- Usage of DB-generated ids and ids generated with Java code(via ReactiveIdentifierGenerator)
- Validation of values
- Attribute converters
- Usage of session opened through openSession() method as well as withSession()
- Usage of transaction started through openSession() method as well as withSession()
- Usage of session and transaction opened with the same method
- Session methods: find, persist, remove, merge, flush
- Queries: regular, native, named, JPA criteria
- Query methods: setParameter, setMaxResults, getFirstResult, getSingleResult, getResultList, executeUpdate
- Eager and lazy fetches
- Field-level lazy fetching(Hibernate team "doesn't encourage" using this feature, though)
- Stateless sessions and their methods

Following test coverage should be implemented for Hibernate Reactive with Panache:
- Active record pattern
- Repository pattern
- list and stream methods
- find method
- sorting
- Named queries
- DTO projection
- Transactions
- Id generation

All tests should run for PostgreSQL, MySQL, DB2, MS SQL and Oracle SQL.

### Impact on testsuites and testing automation:
 - Ensure this coverage works both in JVM and NATIVE mode
 - Some smoke tests were created in "examples" section of quarkus-test-framework, they should be moved into quarkus-test-suite and all subsequent tests should be created there.

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 - Bare metal tests will deploy docker containers with DBs, but they are mostly lightweight
 - Only exception is a DB2 database, which consumes a lot of resources, so there should be an option to disable running tests for it.
 - Test execution will be prolonged, preliminarily estimation is ~10 minutes per each OCP stream
 - QUARKUS-1079 covers both JVM and NATIVE mode, native image imposes multiple times prolonged execution time and increased memory requirements for the build

Our current testing capacity is sufficient to cover this testing, execution time will be increased though.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage

## Advanced topics
    - Locking
    - Multitenancy
    - Cache
    - Tuning and perfomance