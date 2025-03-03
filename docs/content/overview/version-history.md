---
lastmod: 2021-04-21
date: 2017-03-27
menu:
  main:
    parent: getting started
title: Version History
weight: 30
---

# Version History

### 1.3.4

* Improve compatibility with MySQL Server 8.0.24:
  * Ignore new `ER_CLIENT_INTERACTION_TIMEOUT` error packet sent to timed-out connections: [#970](https://github.com/mysql-net/MySqlConnector/issues/970).
  * Known Issue: Connections with `UseCompression=true` may throw a `MySqlProtocolException` when timed out.

### 1.3.3

* Support `Enum` parameters in prepared commands: [#965](https://github.com/mysql-net/MySqlConnector/issues/965).
* Fix `OverflowException` reading `OkPayload`: [#966](https://github.com/mysql-net/MySqlConnector/issues/966).
* Fix internal SQL parsing error with C-style comments.

### 1.3.2

* Fix a bug that could cause a timed-out query to still throw a `QueryInterrupted` `MySqlException` instead of `CommandTimeoutExpired`.

### 1.3.1

* Remove two new `Info` log messages added in 1.3.0: [#956](https://github.com/mysql-net/MySqlConnector/issues/956).
  * The equivalent messages in 1.2.1 were at `Debug` level.
* Make `Adler32` class `internal`.
  * This was not intended to be added to the public API in 1.3.0.

### 1.3.0

* Connections are now reset asynchronously in the background: [#178](https://github.com/mysql-net/MySqlConnector/issues/178).
  * This speeds up `MySqlConnection.Open(Async)` but still cleans up connections between uses.
  * Use `DeferConnectionReset=true` in the connection string to revert to the old behaviour.
  * _Experimental_ Use `ConnectionIdlePingTime=300` in the connection string to avoid any network I/O when retrieving a connection from the pool; this is fastest but may return invalid connections from `Open`. This setting is experimental and may change in the future.
* Change default value of `IgnorePrepare` to `false`: [#929](https://github.com/mysql-net/MySqlConnector/issues/929).
  * Calling `MySqlCommand.Prepare(Async)` will have an effect by default.
* Implement Azure Server Redirection: [#789](https://github.com/mysql-net/MySqlConnector/issues/789).
  * Support community protocol for server redirection: [#945](https://github.com/mysql-net/MySqlConnector/issues/945).
* Support `MemoryStream` as a value for `MySqlParameter.Value`: [#943](https://github.com/mysql-net/MySqlConnector/issues/943).
* Implement `MySqlException.IsTransient`: [#849](https://github.com/mysql-net/MySqlConnector/issues/849).
* Implement `IComparable<MySqlDateTime>` and `IEquatable<MySqlDateTime>` on `MySqlDateTime`.
* **Breaking** Remove `public` constructor for `MySqlConversionException`.
  * This constructor was never intended to be `public`.
* Implement serialization for exceptions.
* Report `CommandTimeoutExpired` consistently: [#939](https://github.com/mysql-net/MySqlConnector/issues/939).
  * This changes the `MySqlException.ErrorCode` from `QueryInterrupted` to `CommandTimeoutExpired`.
* Nagle's Algorithm is disabled on TCP sockets: [#921](https://github.com/mysql-net/MySqlConnector/issues/921).
* Adler32 checksum (for compressed packets) uses hardware acceleration: [#865](https://github.com/mysql-net/MySqlConnector/issues/865).
* Set timeouts for cancellation operations from `CancellationTimeout` connection string option: [#951](https://github.com/mysql-net/MySqlConnector/issues/951).
* Throw `OperationCanceledException` from `OpenAsync` when the `CancellationToken` is cancelled: [#931](https://github.com/mysql-net/MySqlConnector/issues/931).
* Use transaction for 'SHOW WARNINGS': [#918](https://github.com/mysql-net/MySqlConnector/issues/918).
* Improve exception message for unsupported parameter types: [#925](https://github.com/mysql-net/MySqlConnector/issues/925).
* Fix exception in server version parsing: [#934](https://github.com/mysql-net/MySqlConnector/issues/934).
* Fix silent failure to use TLS 1.3 (when explicitly requested) on older frameworks.
* Fix error deserialising `MySqlException.ErrorCode` property.
* Prevent exceptions being thrown from `MySqlTransaction.Dispose`: [#923](https://github.com/mysql-net/MySqlConnector/issues/923).
* Fix nested `MySqlException` (thrown in some scenarios from `ExecuteReader`).
* Use .NET 5.0 methods to load PEM certificates.
* Thanks to [Oleksandr Novak](https://github.com/novak-as) for contributions to this release.

### 1.2.1

* Fix bug in extracting PEM data when there's extra data in the certificate file: [#912](https://github.com/mysql-net/MySqlConnector/issues/912).
* Thanks to [Laurents Meyer](https://github.com/lauxjpn) for contributions to this release.

### 1.2.0

* Add `TlsCipherSuites` connection string option for fine-grained control of TLS cipher suites: [#904](https://github.com/mysql-net/MySqlConnector/issues/904).
  * This option is only supported on Linux when using .NET Core 3.1 or .NET 5.0 or later.
* Fix bug loading GUIDs with `MySqlBulkCopy`.

### 1.1.0

* Support .NET 5.0.
* Cancel query (server-side) when `CommandTimeout` expires: [#455](https://github.com/mysql-net/MySqlConnector/issues/455).
  * Add `CancellationTimeout` connection string option.
  * Implementation details discussed in [this comment](https://github.com/mysql-net/MySqlConnector/issues/455#issuecomment-702424012).
* Return `null` from `MySqlDataReader.GetSchemaTable` when there is no result set: [#877](https://github.com/mysql-net/MySqlConnector/issues/877).
* Make `DisposeAsync` fully async: [#876](https://github.com/mysql-net/MySqlConnector/issues/876).
* Ignore `ObjectDisposedException` thrown in `TryResetConnectionAsync`: [#897](https://github.com/mysql-net/MySqlConnector/pull/897).
* Ignore closed/disposed connections/commands in `MySqlCommand.Cancel`: [#820](https://github.com/mysql-net/MySqlConnector/pull/820).
* Fix bug where sessions could time out if they were opened but no queries were ever executed.
* Thanks to [Dirk Lemstra](https://github.com/dlemstra) and [laurent-jeancler-realist](https://github.com/laurent-jeancler-realist) for contributions to this release.

### 1.0.1

* Support `ENUM` columns that use the `MYSQL_TYPE_ENUM` type in their column metadata: [#850](https://github.com/mysql-net/MySqlConnector/issues/850).
* Allow `MySqlCommand.CommandText` and `.Connection` to be changed while another command is executing: [#867](https://github.com/mysql-net/MySqlConnector/issues/867).
* Make schema collection names (for `MySqlConnection.GetSchema(collectionName)`) case-insensitive: [#852](https://github.com/mysql-net/MySqlConnector/issues/852).
* Fix `MySqlBulkLoader` with Azure Database for MySQL/MariaDB: [#853](https://github.com/mysql-net/MySqlConnector/issues/853).
* Fix bug preventing the retrieval of more than 2^31-1 rows in a single query: [#863](https://github.com/mysql-net/MySqlConnector/issues/863).
* Fix `MySqlParameterCollection.Insert` implementation: [#869](https://github.com/mysql-net/MySqlConnector/issues/869).
* Fix integer overflow in sequence number for compressed packets.
* Fix ZLIB header flags verification for compressed packets.
* Thanks to [akamor](https://github.com/akamor) for contributions to this release.

### 1.0.0

* **Breaking** Change namespace to `MySqlConnector`: [#827](https://github.com/mysql-net/MySqlConnector/issues/827).
* **Breaking** Rename `MySqlClientFactory` to `MySqlConnectorFactory`: [#839](https://github.com/mysql-net/MySqlConnector/issues/839).
* **Breaking** All `MySqlConnectionStringBuilder` string properties return `""` (not `null`) when unset: [#837](https://github.com/mysql-net/MySqlConnector/issues/837).
* **Breaking** Remove `MySqlInfoMessageEventArgs.errors` property; use `.Errors` instead.
* Implement async schema APIs: [#835](https://github.com/mysql-net/MySqlConnector/issues/835).
* Implement `MySqlConnectorFactory.CanCreateXyz` methods: [#838](https://github.com/mysql-net/MySqlConnector/issues/838).
* Add `net5.0` target framework.
* Add `MySqlException.ErrorCode`: [#830](https://github.com/mysql-net/MySqlConnector/issues/830).
* Add `MySqlConnection.ResetConnectionAsync`: [#831](https://github.com/mysql-net/MySqlConnector/issues/831).
* Add documentation at https://mysqlconnector.net/api/ built from XML doc comments: [#827](https://github.com/mysql-net/MySqlConnector/issues/827).
* Allow rows larger than 1 MiB in `MySqlBulkCopy`: [#834](https://github.com/mysql-net/MySqlConnector/issues/834).
* Reduce memory allocations when hashing passwords (during login).

### 0.69.10

* Ignore `ObjectDisposedException` thrown in `TryResetConnectionAsync`: [#897](https://github.com/mysql-net/MySqlConnector/pull/897).
* Thanks to [laurent-jeancler-realist](https://github.com/laurent-jeancler-realist) for contributions to this release.

### 0.69.9

* Return `null` from `MySqlDataReader.GetSchemaTable` when there is no result set: [#877](https://github.com/mysql-net/MySqlConnector/issues/877).

### 0.69.8

* Fix `MySqlBulkLoader` with Azure Database for MySQL/MariaDB: [#853](https://github.com/mysql-net/MySqlConnector/issues/853).
* Make schema collection names (for `MySqlConnection.GetSchema(collectionName)`) case-insensitive: [#852](https://github.com/mysql-net/MySqlConnector/issues/852).

### 0.69.7

* Support `ENUM` columns that use the `MYSQL_TYPE_ENUM` type in their column metadata: [#850](https://github.com/mysql-net/MySqlConnector/issues/850).

### 0.69.6

* Support `GEOMCOLLECTION` data type alias in MySQL Server 8.0: [#845](https://github.com/mysql-net/MySqlConnector/issues/845).

### 0.69.5

* Improve robustness of OK packet parsing: [#842](https://github.com/mysql-net/MySqlConnector/issues/842).

### 0.69.4

* Fix connection pool leak when a failure (e.g., timeout) occurs on a connection: [#836](https://github.com/mysql-net/MySqlConnector/issues/836).

### 0.69.3

* Fix `Failed to read the result set.` error when using `MySqlBulkCopy`: [#780](https://github.com/mysql-net/MySqlConnector/issues/780).
  * The maximum row size supported by `MySqlBulkCopy`is 1 MiB.

### 0.69.2

* Remove `Console.WriteLine` debugging code that was inadvertently added in 0.69.1.

### 0.69.1

* Fix `OverflowException` when calling `MySqlDataReader.GetInt32` on a `DECIMAL` column: [#832](https://github.com/mysql-net/MySqlConnector/issues/832).

### 0.69.0

* **Breaking** Change `MySqlGeometry.Value` from returning `ReadOnlySpan<byte>` to `byte[]`: [#829](https://github.com/mysql-net/MySqlConnector/pull/829).
* Thanks to [Laurents Meyer](https://github.com/lauxjpn) for contributions to this release.

### 0.68.1

* Fix SQL syntax error when calling `BeginTransaction(IsolationLevel.Snapshot, isReadOnly: true);`: [#817](https://github.com/mysql-net/MySqlConnector/issues/817).

### 0.68.0

* Add `MySqlConnection.BeginTransaction` overload with `isReadOnly` parameter: [#817](https://github.com/mysql-net/MySqlConnector/issues/817).
* Support `MySqlCommand.Prepare` for `CommandType.StoredProcedure`: [#742](https://github.com/mysql-net/MySqlConnector/issues/742).

### 0.67.0

* **Breaking** Add `new` implementations of `MySqlCommand.ExecuteReaderAsync` that return `Task<MySqlDataReader>`: [#822](https://github.com/mysql-net/MySqlConnector/issues/822).
* **Breaking** `MySqlBulkCopy.DestinationTableName` must be quoted if it contains reserved keywords or characters: [#818](https://github.com/mysql-net/MySqlConnector/issues/818).
* Automatically create expressions for `BIT` and binary columns in `MySqlBulkCopy`: [#816](https://github.com/mysql-net/MySqlConnector/issues/816).
* Throw an exception from `MySqlBulkCopy` if not all rows were inserted: [#814](https://github.com/mysql-net/MySqlConnector/issues/814).
* Add logging to `MySqlBulkCopy`.
* Detect simple column mapping errors in `MySqlBulkCopy`.

### 0.66.0

* **Breaking** Add `MySqlBulkCopy.RowsCopied` property: [#809](https://github.com/mysql-net/MySqlConnector/issues/809).
  * The `RowsCopied` event is renamed to `MySqlRowsCopied`.
  * The `MySqlRowsCopied` event is no longer guaranteed to be raised at the end of copying.
* Fix `NullReferenceException` when calling a stored procedure with an `ENUM` parameter: [#812](https://github.com/mysql-net/MySqlConnector/issues/812).
* Track `MySqlParameter` name changes (when added to a `MySqlParameterCollection`): [#811](https://github.com/mysql-net/MySqlConnector/issues/811).

### 0.65.0

* Add `ColumnMappings` to `MySqlBulkCopy`: [#773](https://github.com/mysql-net/MySqlConnector/issues/773).

### 0.64.2

* Restore `COLUMN_TYPE` column to `GetSchema("COLUMNS")`: [#807](https://github.com/mysql-net/MySqlConnector/pull/807).
  * This was a regression in 0.64.1
* Fix ignored `CancellationToken` in `MySqlBulkCopy.WriteToServerAsync(DataTable)`.
* Thanks to [mitchydeath](https://github.com/mitchydeath) for contributions to this release.

### 0.64.1

* Fix timeout for named pipe connections: [#804](https://github.com/mysql-net/MySqlConnector/issues/804).
* Fix `ArgumentException` calling `MySqlConnection.GetSchema("COLUMNS")`: [#802](https://github.com/mysql-net/MySqlConnector/issues/802).
* Fix `Unknown column 'SRS_ID'` exception calling `MySqlConnection.GetSchema("COLUMNS")`: [#805](https://github.com/mysql-net/MySqlConnector/issues/805).

### 0.64.0

* Support `TlsVersion` connection string option: [#760](https://github.com/mysql-net/MySqlConnector/issues/760).
* Implement `IConvertible` on `MySqlDateTime`: [#798](https://github.com/mysql-net/MySqlConnector/issues/798).
* Always use `SESSION` transaction isolation level: [#801](https://github.com/mysql-net/MySqlConnector/issues/801).
* Avoid composite commands when starting a transaction: [#774](https://github.com/mysql-net/MySqlConnector/issues/774).

### 0.63.2

* Support `IsolationLevel.Snapshot` in `BeginTransaction`: [#791](https://github.com/mysql-net/MySqlConnector/pull/791).
* Support `DataSourceInformation` in `GetSchema`: [#795](https://github.com/mysql-net/MySqlConnector/pull/795).
* Thanks to [John Battye](https://github.com/battyejp) and [Vincent DARON](https://github.com/vdaron) for contributions to this release.

### 0.63.1

* Fix missing quoting of table name in `MySqlBulkCopy`: [#792](https://github.com/mysql-net/MySqlConnector/issues/792).
* Fix bug in `ChangeDatabase` that rolled back an active transaction: [#794](https://github.com/mysql-net/MySqlConnector/issues/794).

### 0.63.0

* **Experimental** Add new transaction savepoint API (from .NET 5): [#775](https://github.com/mysql-net/MySqlConnector/issues/775).
* Allow `TINYINT(1)` (`BOOL`) columns to be read using `MySqlDataReader.GetInt32`, `GetInt16`, `GetByte`, etc. when `TreatTinyAsBoolean=true`: [#782](https://github.com/mysql-net/MySqlConnector/issues/782).
  * These methods will always return `1` for any non-zero value in the underlying column.
* Allow `FLOAT` and `DOUBLE` columns to be read using `MySqlDataReader.GetDecimal`: [#785](https://github.com/mysql-net/MySqlConnector/pull/785).
* Fix connection timeout when server doesn't respond: [#739](https://github.com/mysql-net/MySqlConnector/issues/739).
* Thanks to [Daniel Cohen Gindi](https://github.com/danielgindi) for contributions to this release.

### 0.62.0

* **Experimental** Add new `MySqlBulkCopy` class for efficiently loading a table from a `DataTable` or `IDataReader`: [#737](https://github.com/mysql-net/MySqlConnector/issues/737)
  * Known issue: individual data values larger than 16MiB cannot be sent.
* Improve nullability annotations.
  * `MySqlCommand.CommandText` defaults to the empty string: [#743](https://github.com/mysql-net/MySqlConnector/issues/743).
  * **Breaking** Return empty schema when there is no result set: [#744](https://github.com/mysql-net/MySqlConnector/issues/744).
* Optimize `MySqlDataReader.GetInt32`: [#725](https://github.com/mysql-net/MySqlConnector/pull/725).
* Set TCP Keepalive for all operating systems: [#746](https://github.com/mysql-net/MySqlConnector/issues/746).
* Remove properties from `MySqlConnectionStringBuilder` when they're set to `null`: [#749](https://github.com/mysql-net/MySqlConnector/issues/749).
* Send shorter connector version to server: [#765](https://github.com/mysql-net/MySqlConnector/issues/765).
* Throw better exception for invalid connection string values: [#763](https://github.com/mysql-net/MySqlConnector/issues/763).
* Fix `KeyNotFoundException` in `GetAndRemoveStream`: [#757](https://github.com/mysql-net/MySqlConnector/issues/757).
* Reduce `ObjectDisposedExceptions` thrown from `MySqlCommand`.

### 0.61.0

* Add `MySqlConnection.CloneWith`: [#736](https://github.com/mysql-net/MySqlConnector/issues/736).

### 0.60.4

* Fix disclosure of connection password via `MySqlConnection.Clone`: [#735](https://github.com/mysql-net/MySqlConnector/issues/735).

### 0.60.3

* Improve detection of Azure Database for MySQL proxy: [#731](https://github.com/mysql-net/MySqlConnector/issues/731).
* Implement `CommandBehavior.SingleResult` and `SingleRow`: [#681](https://github.com/mysql-net/MySqlConnector/issues/681).
* Improve "Connect Timeout" exception message when connection pool is empty.
* Revalidate missing stored procedures in `MySqlCommandBuilder.DeriveParameters(Async)`: [#730](https://github.com/mysql-net/MySqlConnector/issues/730).

### 0.60.2

* Add more schemas to `MySqlConnection.GetSchema`: [#724](https://github.com/mysql-net/MySqlConnector/pull/724).
* Add XML documentation to NuGet package.
* Add documentation for `MySqlConnection.ConnectionTimeout`: [#727](https://github.com/mysql-net/MySqlConnector/pull/727).
* Fix exception in `MySqlDataReader.FieldCount` and `HasRows`: [#728](https://github.com/mysql-net/MySqlConnector/issues/728).
  * This fixes a regression introduced in 0.60.1.
* Thanks to [Joseph Amalfitano](https://github.com/JosephAmalfitanoSSA) and [KaliVi](https://github.com/KaliVi) for contributions to this release.

### 0.60.1

* Implement `CommandBehavior.SchemaOnly`: [#723](https://github.com/mysql-net/MySqlConnector/issues/723).
* Fix `MySqlDataReader` methods returning data for output parameters of stored procedures: [#722](https://github.com/mysql-net/MySqlConnector/issues/722).
  * This fixes a regression introduced in 0.57.0.

### 0.60.0

* **Possibly breaking** Implement conversions in GetFieldValue<T>: [#716](https://github.com/mysql-net/MySqlConnector/issues/716).
* Add C# 8 nullable annotations to public API.
* Support `Tables` and `Views` schemas in `MySqlConnection.GetSchema`: [#719](https://github.com/mysql-net/MySqlConnector/pull/719).
* Add better exception message when `'0000-00-00'` can't be converted: [#690](https://github.com/mysql-net/MySqlConnector/issues/690).
* Implement `MySqlConnection.Clone`: [#720](https://github.com/mysql-net/MySqlConnector/issues/720).
* Update list of reserved words.
* Use new `Socket` async APIs (.NET Standard 2.1, .NET Core 3.0).
* Update `System.Net.Security` dependency to v4.3.1 (.NET Standard 1.3).
* Thanks to [Roman Marusyk](https://github.com/Marusyk) and [KaliVi](https://github.com/KaliVi) for contributions to this release.

### 0.59.2

* Fix error when reading a `BIT(1)` column: [#713](https://github.com/mysql-net/MySqlConnector/issues/707).
  * This fixes a problem introduced in 0.59.1.

### 0.59.1

* Allow `DECIMAL` column type to be read by `GetBoolean`: [#707](https://github.com/mysql-net/MySqlConnector/issues/707).
* Fix error in reading `BIT(n)` columns: [#708](https://github.com/mysql-net/MySqlConnector/issues/708).
* Thanks to [Laurents Meyer](https://github.com/lauxjpn) for contributions to this release.

### 0.59.0

* **Breaking** `MySqlDataReader.GetFloat()` converts `REAL` values (from `double` to `float`) instead of throwing `InvalidCastException`: [#706](https://github.com/mysql-net/MySqlConnector/issues/706).
* Fix error in `mysql_clear_password` authentication plugin: [#703](https://github.com/mysql-net/MySqlConnector/issues/703).
* Thanks to [Laurents Meyer](https://github.com/lauxjpn) for contributions to this release.

### 0.58.0

* Add `NoBackslashEscapes` connection option: [#701](https://github.com/mysql-net/MySqlConnector/issues/701).
* Store icon in the NuGet package: [#705](https://github.com/mysql-net/MySqlConnector/issues/705).
* Thanks to [Laurents Meyer](https://github.com/lauxjpn) for contributions to this release.

### 0.57.0

* **Breaking** Remove `MySqlClientFactory.Register`: [#654](https://github.com/mysql-net/MySqlConnector/issues/654).
  * Replace calls to this method with `DbProviderFactories.RegisterFactory("MySqlConnector", MySqlClientFactory.Instance)` instead.
* **Breaking** Return type of `MySqlConnection.BeginTransactionAsync` changed to `ValueTask<MySqlTransaction>` (to match .NET Core 3.0 APIs).
* **Breaking** Various `XyzAsync` method overloads that did not take a `CancellationToken` were removed.
* **Breaking** Throw `InvalidOperationException` from `MySqlDataReader.GetSchemaTable` when there is no result set: [#678](https://github.com/mysql-net/MySqlConnector/issues/678).
* **Experimental** Implement the new ADO.NET `DbBatch` API: [#650](https://github.com/mysql-net/MySqlConnector/issues/650).
  * This API is not finalised and may change in the future.
* Add `netstandard2.1` and `netcoreapp3.0` platforms.
* Implement .NET Core 3.0 ADO.NET API.
* Add .NET Core 3.0 async methods: [#642](https://github.com/mysql-net/MySqlConnector/issues/642).
* Allow `MySqlDataReader.GetDouble` and `GetFloat` on `DECIMAL` columns: [#664](https://github.com/mysql-net/MySqlConnector/issues/664).
* Allow narrowing conversions in `MySqlDataReader.GetByte`: [#695](https://github.com/mysql-net/MySqlConnector/issues/695).
* Add `MySqlGeometry` and `MySqlDataReader.GetMySqlGeometry`: [#677](https://github.com/mysql-net/MySqlConnector/issues/677).
  * The API is deliberately different than Connector/NET, which assumes a `MySqlGeometry` can only be a simple point.
* Use `sql_select_limit` when `CommandBehavior.SingleRow` is specified: [#679](https://github.com/mysql-net/MySqlConnector/issues/679).
* Use batching in `MySqlDataAdapter` when `UpdateBatchSize` is set: [#675](https://github.com/mysql-net/MySqlConnector/issues/675).
* Support `utf8mb4_0900_bin` collation introduced in MySQL Server 8.0.17: [#670](https://github.com/mysql-net/MySqlConnector/issues/670).
* Add `MySqlConnection.CloseAsync`: [#467](https://github.com/mysql-net/MySqlConnector/issues/467).
* Throw `InvalidOperationException` from `MySqlConnection.EnlistTransaction` instead of `NullReferenceException`.
* Fix `NullReferenceException` thrown from `MySqlConnection.ConnectionTimeout`: [#669](https://github.com/mysql-net/MySqlConnector/issues/669).
* Fix connection timeout when executing a stored procedure: [#672](https://github.com/mysql-net/MySqlConnector/issues/672).
* Fix incorrect exception being thrown after a timeout occurs executing a stored procedure: [#667](https://github.com/mysql-net/MySqlConnector/issues/667).
* Fix exception deserializing an `OUT BOOL` parameter from a stored procedure: [#682](https://github.com/mysql-net/MySqlConnector/issues/682).
* Fix exception deserializing an `OUT TIME` parameter from a stored procedure: [#680](https://github.com/mysql-net/MySqlConnector/issues/680).
* Fix `MySqlConnection.State` not being set to `ConnectionState.Closed` when a failure occurs if pooling is disabled: [#674](https://github.com/mysql-net/MySqlConnector/issues/674).
* Fix exception when executing a prepared statement if `MySqlParameter.MySqlDbType` was set: [#659](https://github.com/mysql-net/MySqlConnector/issues/659).
* Handle error packet being sent out-of-order: [#662](https://github.com/mysql-net/MySqlConnector/issues/662).
* Use `MySqlErrorCode.UnableToConnectToHost` in more situations when connecting fails: [#647](https://github.com/mysql-net/MySqlConnector/issues/647).
* Add some nullable annotations; these are primarily on internal types and not in the public API.
* Reduce allocations on some common code paths.
* Improve performance of `MySqlDataReader`; reduce memory allocations.
* Thanks to [Josh Rees](https://github.com/joshdrees) for contributions to this release.

### 0.56.0

* Support `client_ed25519` authentication plugin for MariaDB: [#639](https://github.com/mysql-net/MySqlConnector/issues/639).
  * This is implemented in a new NuGet package, [MySqlConnector.Authentication.Ed25519](https://www.nuget.org/packages/MySqlConnector.Authentication.Ed25519/), and must be activated by calling `Ed25519AuthenticationPlugin.Install`.

### 0.55.0

* **Breaking** `MySqlBulkLoader` (for local files) and `LOAD DATA LOCAL INFILE` are disabled by default.
  * Set `AllowLoadLocalInfile=true` in the connection string to enable loading local data.
  * This is a security measure; see https://fl.vu/mysql-load-data for details.
* Add `AllowLoadLocalInfile` connection string option: [#643](https://github.com/mysql-net/MySqlConnector/issues/643).
* Add `SslCert` and `SslKey` connection string options to specify a client certificate using PEM files: [#641](https://github.com/mysql-net/MySqlConnector/issues/641).
* Add `SslCa` alias for the `CACertificateFile` connection string option: [#640](https://github.com/mysql-net/MySqlConnector/issues/640).

### 0.54.0

* Implement batch updates in `MySqlDataAdapter`: [#635](https://github.com/mysql-net/MySqlConnector/issues/635).
  * See [Performing Batch Operations Using DataAdapters](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/performing-batch-operations-using-dataadapters) for information about the API.
* Improve compatibility with latest Azure Database for MySQL changes.

### 0.53.0

* **Breaking** `MySqlDataReader.GetTextReader()` will throw an `InvalidCastException` if the field value is NULL. Previously, it would return a `StringReader` wrapping an empty string.
* Add `MySqlDataReader.GetTextReader(string name)`.
* Implement `MySqlDataReader.GetFieldValue<T>` for `TextReader` and `Stream`.

### 0.52.0

* **Potentially breaking** Change default connection collation from `utf8mb4_general_ci` to the server's default for `utf8mb4`: [#626](https://github.com/mysql-net/MySqlConnector/issues/626).
  * This updates a change made in 0.48.0.
* Fix "Command timeout" exception being thrown when there wasn't a command timeout: [#628](https://github.com/mysql-net/MySqlConnector/issues/628).

### 0.51.1

* Add support for `Memory<byte>` and `ArraySegment<byte>` as `MySqlParameter.Value` values.
* Fix exception when setting `MySqlParameter.Value` to `ReadOnlyMemory<byte>` when using prepared commands.

### 0.51.0

* Set `MySqlException.Number` to `MySqlErrorCode.UnableToConnectToHost` in more situations when connecting times out: [#622](https://github.com/mysql-net/MySqlConnector/pull/622).
* Improve handling of `MySqlConnection.Close()` within `TransactionScope`: [#620](https://github.com/mysql-net/MySqlConnector/issues/620).
* Allow `MySqlParameter.Value` to be a `ReadOnlyMemory<byte>`: [#624](https://github.com/mysql-net/MySqlConnector/issues/624).
* Thanks to <a href="https://github.com/mguinness">mguinness</a> for contributions to this release.

### 0.50.0

* Add `MySqlClientFactory.Register()` for integration with `DbProviderFactories` in `netcoreapp2.1`: [#526](https://github.com/mysql-net/MySqlConnector/issues/526).
* Use more efficient "Reset Connection" for MariaDB 10.2.4 and later: [#613](https://github.com/mysql-net/MySqlConnector/issues/613).
* Ignore `MySqlConnection.EnlistTransaction` called more than once for the same transaction: [#619](https://github.com/mysql-net/MySqlConnector/issues/619).
* `MySqlConnection.ConnectionString` will always be coerced from `null` to the empty string.
* Use `ReadOnlySpan<byte>` in more places when parsing server responses.
* Fix multiple `NullReferenceException` errors that could occur in edge cases.

### 0.49.3

* Use correct isolation level when starting a transaction for `System.Transactions.TransactionScope`: [#605](https://github.com/mysql-net/MySqlConnector/issues/605).

### 0.49.2

* Fix bug in parsing OK packet when `CLIENT_SESSION_TRACK` isn't supported: [#603](https://github.com/mysql-net/MySqlConnector/pull/603).

### 0.49.0

* **Breaking** The default value for the `UseAffectedRows` connection string option has changed from `true` to `false`. This provides better compatibility with Connector/NET's defaults and also with other ADO.NET libraries: [#600](https://github.com/mysql-net/MySqlConnector/issues/600).
  * If you are upgrading from an earlier version of MySqlConnector, either audit your uses of the return value of `ExecuteNonQuery` (it will now return the number of rows matched by the `WHERE` clause for `UPDATE` statements, instead of the number of rows whose values are actually changed), or add `UseAffectedRows=true` to your connection string.
  * If you are migrating (or have recently migrated) from Connector/NET to MySqlConnector, then no changes need to be made: MySqlConnector now exhibits the same default behaviour as Connector/NET.
* Make `MySqlException` serializable: [#601](https://github.com/mysql-net/MySqlConnector/issues/601).
* Set `MySqlException.Number` to `MySqlErrorCode.UnableToConnectToHost` when connecting fails: [#599](https://github.com/mysql-net/MySqlConnector/issues/599).
* Populate `MySqlException.Data` dictionary: [#602](https://github.com/mysql-net/MySqlConnector/issues/602).

### 0.48.2

* Fix `InvalidCastException` in `MySqlDataReader.GetDateTime` when `AllowZeroDateTime=True`: [#597](https://github.com/mysql-net/MySqlConnector/issues/597).

### 0.48.1

* Add `net471` as target platform: [#595](https://github.com/mysql-net/MySqlConnector/issues/595).
* Support `IDbColumnSchemaGenerator` interface in `netcoreapp2.1` package.
* Fix error in binding parameter values for prepared statements.
* Fix exception when using more than 32,767 parameters in a prepared statement.

### 0.48.0

* **Breaking** Disallow duplicate parameter names after normalization: [#591](https://github.com/mysql-net/MySqlConnector/issues/591).
* **Potentially breaking** Change default connection collation from `utf8mb4_bin` to `utf8mb4_general_ci`: [#585](https://github.com/mysql-net/MySqlConnector/issues/585).
* **Potentially breaking** Update stored procedure metadata cache to use `mysql.proc` when available: [#569](https://github.com/mysql-net/MySqlConnector/issues/569).
  * This provides higher performance, but is a potentially-breaking change for any client using stored procedures.
* Change `System.Transactions` support:
  * Add `UseXaTransactions` connection string option to opt out of XA transactions (equivalent to Connector/NET behaviour): [#254](https://github.com/mysql-net/MySqlConnector/issues/254).
  * **Potentially breaking** Opening multiple (distinct) `MySqlConnection` objects within the same transaction will reuse the same server session: [#546](https://github.com/mysql-net/MySqlConnector/issues/546).
* Add `MySqlConnection.InfoMessage` event: [#594](https://github.com/mysql-net/MySqlConnector/issues/594).
* Implement `ICloneable` on `MySqlCommand`: [#583](https://github.com/mysql-net/MySqlConnector/issues/583).
* Fix logic for detecting variable names in SQL: [#195](https://github.com/mysql-net/MySqlConnector/issues/195), [#589](https://github.com/mysql-net/MySqlConnector/issues/589).
* Fix `NullReferenceException` when attempting to invoke a non-existent stored procedure.
* Support MySQL Server 5.1 (and earlier) by using `utf8` if `utf8mb4` isn't available.
* Reduce log message severity for session discarded due to `ConnectionLifeTime`: [#586](https://github.com/mysql-net/MySqlConnector/issues/586).
* Optimise `MySqlDataReader.GetStream`: [#592](https://github.com/mysql-net/MySqlConnector/issues/592).
* Use latest dotnet SourceLink package.

### 0.47.1

* Fix `NullReferenceException` in `GetSchemaTable`: [#580](https://github.com/mysql-net/MySqlConnector/issues/580).
* Return correct schema table for second result set: [#581](https://github.com/mysql-net/MySqlConnector/issues/581).

### 0.47.0

* Support <a href="https://mariadb.com/kb/en/library/authentication-plugin-gssapi/">MariaDB GSSAPI</a> authentication: [#577](https://github.com/mysql-net/MySqlConnector/pull/577).
* Log received error payloads at `Debug` level.
* Thanks to <a href="https://github.com/vaintroub">Vladislav Vaintroub</a> for contributions to this release.

### 0.46.2

* Fix missing `InnerException` on `MySqlConnection` created for a timeout: [#575](https://github.com/mysql-net/MySqlConnector/issues/575).

### 0.46.1

* Fix `CryptographicException` when loading a PFX certificate file: [#574](https://github.com/mysql-net/MySqlConnector/issues/574).

### 0.46.0

* Add `MySqlParameter.Clone`.
* Implement `ICloneable` on `MySqlParameter`: [#567](https://github.com/mysql-net/MySqlConnector/pull/567).
* Implement `MySqlParameterCollection.CopyTo`.
* Add logging for cached procedures.
* Thanks to [Jorge Rocha Gualtieri](https://github.com/jrocha) for contributions to this release.

### 0.45.1

* Fix error parsing SQL parameters: [#563](https://github.com/mysql-net/MySqlConnector/issues/563).
* Add documentation for common errors: [#565](https://github.com/mysql-net/MySqlConnector/issues/565).

### 0.45.0

* Implement `MySqlConnection.GetSchema("ReservedWords")`: [#559](https://github.com/mysql-net/MySqlConnector/issues/559).
* Optimisation: Use `ReadOnlySpan<byte>` when deserialising payloads.
* Thanks to [Federico Sasso](https://github.com/fedesasso) for contributions to this release.

### 0.44.1

* `MySqlCommand.Prepare` will cache the prepared command until the connection is reset.
* Improve performance of `MySqlCommand.Prepare`, especially when preparation is unnecessary.
* Lazily allocate `MySqlParameterCollection` (accessed via `MySqlCommand.Parameters`) for better performance when command parameters aren't used.
* Use `GC.SuppressFinalize` to improve performance when various objects (derived from `Component`) aren't properly disposed.

### 0.44.0

* Add `Application Name` connection string setting: [#547](https://github.com/mysql-net/MySqlConnector/pull/547).
* Clear connection pools on exit: [#545](https://github.com/mysql-net/MySqlConnector/pull/545).
* Allow `ConnectionString` to be set on a closed connection: [#543](https://github.com/mysql-net/MySqlConnector/pull/543).
* Fix intermittent `NotSupportedException` with SSL connections under .NET Core 2.1: [#509](https://github.com/mysql-net/MySqlConnector/pull/509).

### 0.43.0

* Add first version of prepared commands: [#534](https://github.com/mysql-net/MySqlConnector/pull/534).
  * Only single statements (and not stored procedures) are preparable.
  * The (new) `IgnorePrepare` connection string option defaults to `true` and must be set to `false` to use prepared commands.
* Add `CertificateStore` and `CertificateThumbprint` connection string options: [#536](https://github.com/mysql-net/MySqlConnector/issues/536).
* Fix bug that rejected sessions from the connection pool if `ChangeDatabase` had been called: [#515](https://github.com/mysql-net/MySqlConnector/issues/515).
* Don't map `TINYINT(1) UNSIGNED` as `bool`: [#530](https://github.com/mysql-net/MySqlConnector/issues/530).
* Thanks to [Jan Hajek](https://github.com/hajekj) for contributions to this release.

### 0.42.3

* Fix bug (introduced in 0.42.2) that caused extremely high memory usage: [#528](https://github.com/mysql-net/MySqlConnector/issues/528).
* Allow `DATE` columns with invalid `DateTime` values to be read as `MySqlDateTime`: [#529](https://github.com/mysql-net/MySqlConnector/issues/529).

### 0.42.2

* Fix bug that ignored last result set returned from a stored procedure if it was empty: [#524](https://github.com/mysql-net/MySqlConnector/issues/524).
* Fix extra memory usage if column definition payloads exceeded original size estimate.

### 0.42.1

* Fix `NotImplementedException` reading a `GEOMETRY` column as `byte[]`: [#70](https://github.com/mysql-net/MySqlConnector/issues/70).
* Fix negative `TIME` parsing: [#518](https://github.com/mysql-net/MySqlConnector/issues/518).
* Fix `ArgumentException` preparing a SQL statement: [#520](https://github.com/mysql-net/MySqlConnector/issues/520).

### 0.42.0

* Add `AllowZeroDateTime` connection string option: [#507](https://github.com/mysql-net/MySqlConnector/issues/507).
* Add `MySqlDateTime` type (to allow invalid `DateTime` values to be represented).

### 0.41.0

* Add `GuidFormat` connection string option (for better compatibility with MySQL 8.0 [`UUID_TO_BIN`](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid-to-bin)): [#497](https://github.com/mysql-net/MySqlConnector/issues/497).
* Add `InteractiveSession` connection string option: [#510](https://github.com/mysql-net/MySqlConnector/issues/510).
* Add `PipeName` connection string option: [#454](https://github.com/mysql-net/MySqlConnector/issues/454).
* Improve performance by using `ReadOnlySpan<byte>`, `Utf8Formatter`, and `Utf8Parser`: [#426](https://github.com/mysql-net/MySqlConnector/issues/426).
* Add `netcoreapp2.1` package.
* Remove unnecessary dependencies from `netstandard2.0` package.
* Fix error in loading multiple certificates on macOS.
* Fix bug in setting `MySqlParameter.Precision` and `Scale` under `netstandard1.3`.
* Thanks to [Ed Ball](https://github.com/ejball) for contributions to this release.

### 0.40.4

* Allow `CACertificateFile` to contain multiple (concatenated) certificates: [#499](https://github.com/mysql-net/MySqlConnector/issues/499).
* Avoid `IndexOutOfRangeException` in `ValidateRemoteCertificate`: [#498](https://github.com/mysql-net/MySqlConnector/issues/498).

### 0.40.3

* Fix `caching_sha2_password` authentication for MySQL Server 8.0.11 release: [#489](https://github.com/mysql-net/MySqlConnector/issues/489).
  * **Breaking** This authentication plugin can no longer be used with MySQL Server 8.0.4; you must update to the GA release.

### 0.40.2

* Reverted `ConnectionReset=true` optimisation (added in 0.40.0) that was incompatible with Aurora: [#486](https://github.com/mysql-net/MySqlConnector/issues/486).

### 0.40.1

* Always use new `DateTimeKind` setting: [#484](https://github.com/mysql-net/MySqlConnector/pull/484).

### 0.40.0

* Add `DateTimeKind` connection string setting: [#479](https://github.com/mysql-net/MySqlConnector/pull/479).
* Fix `ArgumentException` for `SslProtocols.None`: [#482](https://github.com/mysql-net/MySqlConnector/issues/482).
* Fix `IOException` being thrown at startup: [#475](https://github.com/mysql-net/MySqlConnector/issues/475).
* Fix race condition in `OpenTcpSocketAsync`: [#476](https://github.com/mysql-net/MySqlConnector/issues/476).
* Optimise `MySqlConnection.Open` when `ConnectionReset=true` (default): [#483](https://github.com/mysql-net/MySqlConnector/pull/483).
* Thanks to [Ed Ball](https://github.com/ejball) for contributions to this release.

#### MySqlConnector.Logging.Microsoft.Extensions.Logging

* Reduce dependencies to just `Microsoft.Extensions.Logging.Abstractions`.

### 0.39.0

* Add `IgnoreCommandTransaction` connection string setting: [#474](https://github.com/mysql-net/MySqlConnector/issues/474).

### 0.38.0

* Add Serilog logging provider: [#463](https://github.com/mysql-net/MySqlConnector/issues/463).
* Add NLog logging provider: [#470](https://github.com/mysql-net/MySqlConnector/pull/470).
* Implement `IDbDataParameter` on `MySqlParameter`: [#465](https://github.com/mysql-net/MySqlConnector/issues/465).
* Implement `MySqlDataReader.GetChar`: [#456](https://github.com/mysql-net/MySqlConnector/issues/456).
* Add `MySqlDataReader.GetFieldType(string)` overload: [#440](https://github.com/mysql-net/MySqlConnector/issues/440).
* Fix a connection pooling session leak in high contention scenarios: [#469](https://github.com/mysql-net/MySqlConnector/issues/469).
* Fix overhead of extra connection pools created in a race: [#468](https://github.com/mysql-net/MySqlConnector/issues/468).
* Thanks to [Marc Lewandowski](https://github.com/marcrocny) and [Rolf Kristensen](https://github.com/snakefoot) for contributions to this release.

### 0.37.1

* Serialize enum parameter values as strings when `MySqlParameter.MySqlDbType` is set to `MySqlDbType.String` or `VarChar`: [#459](https://github.com/mysql-net/MySqlConnector/issues/459).
* Require `ConnectionIdlePingTime` (added in 0.37.0) to be explicitly set to a non-zero value to avoid pinging the server.
* Thanks to [Naragato](https://github.com/naragato) for contributions to this release.

### 0.37.0

* Support TLS 1.2 on Windows clients: [#458](https://github.com/mysql-net/MySqlConnector/pull/458).
  * Use the best TLS version supported by the OS, as per [best practices](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls).
* Add `ConnectionIdlePingTime` connection string setting _(Experimental)_: [#461](https://github.com/mysql-net/MySqlConnector/pull/461).
* Fix failure to log in for accounts with empty passwords that use `caching_sha2_password`.
* Throw `MySqlException` for invalid port number in connection string.

### 0.36.1

* Reap connections more frequently if `ConnectionIdleTimeout` is low: [#442](https://github.com/mysql-net/MySqlConnector/issues/442).
* Fix `ArgumentOutOfRangeException` when command times out: [#447](https://github.com/mysql-net/MySqlConnector/issues/447).
* Speed up connection pooling (particularly in applications that only have one connection string).
* Reduce heap allocations in common scenarios.

### 0.36.0

* **Breaking** Require `CertificateFile` to include private key (for mutual authentication): [#436](https://github.com/mysql-net/MySqlConnector/issues/436).
* Add `MySqlDataReader.GetX(string)` overloads: [#435](https://github.com/mysql-net/MySqlConnector/issues/435).
* Add `MySqlDataReader.GetTimeSpan`: [#438](https://github.com/mysql-net/MySqlConnector/issues/438).
* Add `MySqlDataReader.GetUInt16`, `GetUInt32`, `GetUInt64`: [#439](https://github.com/mysql-net/MySqlConnector/issues/439).
* Set `MySqlConnection.State` to `ConnectionState.Closed` when the connection fails: [#433](https://github.com/mysql-net/MySqlConnector/issues/433).
* Fix error parsing `--` comments in SQL: [#429](https://github.com/mysql-net/MySqlConnector/issues/429).
* Fix error parsing C-style comments in SQL.
* **Breaking** Wrap unhandled `SocketException` in `MySqlException`: [#434](https://github.com/mysql-net/MySqlConnector/issues/434).

### 0.35.0

* Add `MySqlCommandBuilder.DeriveParameters`: [#419](https://github.com/mysql-net/MySqlConnector/issues/419).
* Add `MySqlConnection.Ping` and `MySqlConnection.PingAsync`: [#260](https://github.com/mysql-net/MySqlConnector/issues/260).

### 0.34.2

* Fix exception when a stored procedure returns `NULL` for an `OUT` parameter: [#425](https://github.com/mysql-net/MySqlConnector/issues/425).

### 0.34.1

* Add overloads of `MySqlParameterCollection.Add`: [#424](https://github.com/mysql-net/MySqlConnector/issues/424).
* Fix conversion of `MySqlCommand.LastInsertedId`: [#422](https://github.com/mysql-net/MySqlConnector/issues/422).
* Fix "Expected state to be Failed but was Connected" `InvalidOperationException`: [#423](https://github.com/mysql-net/MySqlConnector/issues/423).
* Improve performance when calling stored procedures with no parameters (this was regressed in 0.34.0).
* Reduce severity of some logging statements.

### 0.34.0

* Implement `MySqlCommandBuilder`: [#303](https://github.com/mysql-net/MySqlConnector/issues/303).
* Add `Microsoft.Extensions.Logging` provider: [#418](https://github.com/mysql-net/MySqlConnector/issues/418).
* Add new `MySqlTransaction.Connection` property that returns an object typed as `MySqlConnection`.
* Support `CLIENT_SESSION_TRACK` protocol option: [#323](https://github.com/mysql-net/MySqlConnector/issues/323).
* Optimization: move procedure cache to connection pool: [#415](https://github.com/mysql-net/MySqlConnector/issues/415).
* Ignore extra data at end of column definition payload: [#413](https://github.com/mysql-net/MySqlConnector/pull/413).
* Handle failure to find procedure: [#282](https://github.com/mysql-net/MySqlConnector/issues/282).
* **Breaking** Clear `MySqlTransaction.Connection` when transaction is committed: [#61](https://github.com/mysql-net/MySqlConnector/issues/61).
  * This is a breaking API change from Connector/NET, but matches other ADO.NET connectors.

### 0.33.2

* **Breaking** Throw `InvalidCastException` instead of `MySqlException` from `MySqlDataReader.GetGuid`.
  * This is a breaking API change from Connector/NET, but matches other ADO.NET connectors.
* Fix default values of `MySqlParameter.ParameterName` and `.SourceColumn`; they now follow MSDN documentation.
* Fix `ObjectDisposedException` when a connection is returned to the pool: [#411](https://github.com/mysql-net/MySqlConnector/issues/411).
* Fix `NotSupportedException` when `MySqlParameter.Value` is set to a `char`: [#412](https://github.com/mysql-net/MySqlConnector/issues/412).

### 0.33.1

* Add missing `.ConfigureAwait(false)`
  * Fixes a potential deadlock in clients that blocked on `Task`s returned from async methods.

### 0.33.0

* Implement logging framework: [#390](https://github.com/mysql-net/MySqlConnector/issues/390).
  * Add new project, `MySqlConnector.Logging.log4net`, that adapts MySqlConnector logging for log4net.
* Implement `MySqlDataAdapter`: [#183](https://github.com/mysql-net/MySqlConnector/issues/183).
* Get correct connection ID for Azure Database for MySQL.
* Use `AdoNet.Specification.Tests` test suite to validate implementation.

### 0.32.0

* Implement more `MySqlParameter` constructor overloads: [#402](https://github.com/mysql-net/MySqlConnector/issues/402).
  * This improves compatibility with Connector/NET.
* Implement `MySqlParameter.Precision` and `MySqlParameter.Scale`.
  * The properties are provided only for source compatibility.
  * Not available on .NET 4.5.
* Implement `MySqlDataReader.GetChars`.
* Implement `MySqlDataReader.Depth`.
* Fix `NullReferenceException` in `MySqlDataReader` when reader is disposed.
* **Breaking** Throw `InvalidCastException` (instead of `MySqlException`) from `MySqlDataReader.GetGuid` if column is `NULL`.
* **Breaking** Throw `InvalidOperationException` (instead of `MySqlException`) from `MySqlConnection.ConnectionString` setter if connection is open.
* **Breaking** Throw `ArgumentException` (instead of `InvalidOperationException`) from `MySqlConnectionStringBuilder` for invalid option names.

### 0.31.3

* Fix return value of `ExecuteScalar` to be the first column from the first row of the first result set.
* Fix return value of `ExecuteNonQuery` to correctly return -1 for `SELECT` statements.
* Fix bug where `NextResult` returns `true` for a trailing comment in a SQL statement.
* **Breaking** Throw `InvalidOperationException` if `MySqlCommand.CommandText` is set while the command is active.
* **Breaking** Throw `InvalidOperationException` (instead of `MySqlException`) if a `MySqlCommand` is executed while there is an open reader.
* **Breaking** Throw `InvalidOperationException` from `MySqlCommand.Prepare` when preconditions aren't met.

### 0.31.2

* **Breaking** Throw `InvalidOperationException` when `MySqlCommand.Connection` can't be set (instead of `MySqlException`).
* **Breaking** Throw `InvalidOperationException` from `MySqlCommand.Prepare` when preconditions aren't met.
* Fix `NullReferenceException` when `MySqlCommand.Connection` isn't set (now correctly throws `InvalidOperationException`).

### 0.31.1

* Fix `InvalidOperationException` if `MySqlBulkLoader` is used inside a transaction (again): [#300](https://github.com/mysql-net/MySqlConnector/issues/300).
* **Breaking** Remove `MySqlBulkLoader.Transaction` property (added in 0.24.0); `MySqlBulkLoader` will always use the ambient transaction, if any. This matches Connector/NET API & behaviour.

### 0.31.0

* Implement `MinimumPoolSize`: [#85](https://github.com/mysql-net/MySqlConnector/issues/85).
* Implement server load balancing with new `LoadBalance` connection string setting: [#226](https://github.com/mysql-net/MySqlConnector/issues/226).
* Add SourceLink.
* Wrap `EndOfStreamException` in `MySqlException` when connecting fails: [#388](https://github.com/mysql-net/MySqlConnector/issues/388).
* Fix `StackOverflowException` when reading large BLOBs asynchronously.
* Don't set `Transaction` on new `MySqlCommand`: [#389](https://github.com/mysql-net/MySqlConnector/issues/389).
* Ignore `MySqlConnection.Cancel` when connection is broken: [#386](https://github.com/mysql-net/MySqlConnector/issues/386).
* Improve internal code organisation: [#376](https://github.com/mysql-net/MySqlConnector/issues/376).

### 0.30.0

* **Breaking** Remove `BufferResultSets` connection string option: [#378](https://github.com/mysql-net/MySqlConnector/pull/378).
* The assembly is now strong-named: [#224](https://github.com/mysql-net/MySqlConnector/issues/224).

### 0.29.4

* Fix exception in `MySqlTransaction.Dispose` if the underlying connection is closed or faulted: [#383](https://github.com/mysql-net/MySqlConnector/issues/383).

### 0.29.3

* Remove `System.Runtime.InteropServices.RuntimeInformation` dependency on full framework: [#381](https://github.com/mysql-net/MySqlConnector/issues/381).

### 0.29.2

* Fix an exception if `MySqlDataReader.GetOrdinal` was called before `Read`: [#379](https://github.com/mysql-net/MySqlConnector/issues/379).

### 0.29.1

* Work around Amazon Aurora `DateTime` conversion issue: [#364](https://github.com/mysql-net/MySqlConnector/issues/364).
* Fix `NotSupportedException` in `MySqlParameter`: [#367](https://github.com/mysql-net/MySqlConnector/pull/367).
* **Breaking** Remove a number of `MySqlErrorCode` enum values (to reduce library size).
* Thanks to [Duane Gilbert](https://github.com/dgilbert) and [Naragato](https://github.com/Naragato) for contributions to this release.

### 0.29.0

* **Breaking** Implement `MySqlConnectionStringBuilder.DefaultCommandTimeout` and `MySqlCommand.CommandTimeout` with a default of 30 seconds: [#67](https://github.com/mysql-net/MySqlConnector/issues/67).
  * This may cause long-running queries to throw an exception instead of succeeding; as a workaround, increase `CommandTimeout`.
* Expose `MySqlDbType` and `MySqlCommand.MySqlDbType`: [#362](https://github.com/mysql-net/MySqlConnector/issues/362).
  * MySqlConnector adds `MySqlDbType.Bool` to represent a `TINYINT(1)` column.
  * Return correct values for `ProviderType` in `GetColumnSchema`/`GetSchemaTable`.
* Implement `MySqlConnection.GetSchema`: [#361](https://github.com/mysql-net/MySqlConnector/issues/361).
* Update documentation for .NET Core 2.0: [#372](https://github.com/mysql-net/MySqlConnector/issues/372).
* Fix information disclosure vulnerability related to `LOAD DATA LOCAL INFILE`: [#334](https://github.com/mysql-net/MySqlConnector/issues/334).
* Improve async performance.
* Throw exception for unexpected API use: [#308](https://github.com/mysql-net/MySqlConnector/issues/308).
* Thanks to [Gabden Ayazbayev](https://github.com/Drake103), [Tuomas Hietanen](https://github.com/Thorium), and [Dustin Masters](https://github.com/dustinsoftware) for contributions to this release.

### 0.28.2

* Allow the auth plugin name in the initial handshake to be EOF-terminated: [#351](https://github.com/mysql-net/MySqlConnector/issues/351).

### 0.28.1

* Fix garbage data being returned by `GetColumnSchema`/`GetSchemaTable`: [#354](https://github.com/mysql-net/MySqlConnector/issues/354).
* Fix incorrect `NumericPrecision` for `decimal(n,0)` columns: [#356](https://github.com/mysql-net/MySqlConnector/issues/356).

### 0.28.0

* Support `caching_sha2_password` authentication for MySQL 8.0: [#329](https://github.com/mysql-net/MySqlConnector/issues/329).
* Fix inconsistent return value of `MySqlDataReader.HasRows`: [#348](https://github.com/mysql-net/MySqlConnector/issues/348).
* Thanks to [Drake103](https://github.com/Drake103) for contributions to this release.

### 0.27.0

* Implement `MySqlDataReader.GetColumnSchema`: [#182](https://github.com/mysql-net/MySqlConnector/issues/182).
* Implement `MySqlDataReader.GetSchemaTable`: [#307](https://github.com/mysql-net/MySqlConnector/issues/307).
* Support MySQL Server 8.0.3 and MariaDB 10.2 collations: [#336](https://github.com/mysql-net/MySqlConnector/issues/336), [#337](https://github.com/mysql-net/MySqlConnector/issues/337), [#338](https://github.com/mysql-net/MySqlConnector/issues/338).
* Reduce allocations to improve performance: [#342](https://github.com/mysql-net/MySqlConnector/pull/342), [#343](https://github.com/mysql-net/MySqlConnector/pull/343).
* Thanks to [Alex Lee](https://github.com/elemount) and [Dave Dunkin](https://github.com/ddunkin) for contributions to this release.

### 0.26.5

* Fix hang closing connection with ClearDB on Azure: [#330](https://github.com/mysql-net/MySqlConnector/issues/330).
* Thanks to [Marcin Badurowicz](https://github.com/ktos) for contributions to this release.

### 0.26.4

* Fix overly-broad exception handler introduced in 0.26.3.
* Improve efficiency of code added in 0.26.3.

### 0.26.3

* Fix `HasRows` incorrectly returning `false` after all rows have been read: [#327](https://github.com/mysql-net/MySqlConnector/issues/327).
* Fix `EndOfStreamException` when reusing a pooled connection with Amazon Aurora.
* Reduce network roundtrips when opening a pooled connection (with the default settings of `Pooling=True;Connection Reset=true`); see [#258](https://github.com/mysql-net/MySqlConnector/issues/258).
* Update `System.*` dependencies to 4.3.0 for .NET 4.5 and .NET 4.6 packages.
* Thanks to [Brad Nabholz](https://github.com/bnabholz) for contributions to this release.

### 0.26.2

* Support `CLIENT_DEPRECATE_EOF` flag: [#322](https://github.com/mysql-net/MySqlConnector/issues/322).
* Throw better exception when a malformed packet is detected.
* Don't allow sessions in an error state to be put back into the pool.
* Remove unsupported `CLIENT_PS_MULTI_RESULTS` flag (sent during connection handshaking).

### 0.26.1

* Throw better exception when MySQL Server sends an old authentication method switch request packet: [#316](https://github.com/mysql-net/MySqlConnector/pull/316).
* Capture InnerException in `ActivateResultSet`.
* Thanks to [kobake](https://github.com/kobake) for contributions to this release.

### 0.26.0

* Add convenience methods that return derived types: [#313](https://github.com/mysql-net/MySqlConnector/issues/313).

### 0.25.1

* Prevent exception being thrown from `MySqlSession.DisposeAsync`, which could cause leaked connections: [#305](https://github.com/mysql-net/MySqlConnector/issues/305).

### 0.25.0

* Add `netstandard2.0` compile target: [#270](https://github.com/mysql-net/MySqlConnector/issues/270).

### 0.24.2

* Fix leaked session when a `MySqlException` is thrown because a query contains a user-defined variable and `Allow User Variables=false`: [#305](https://github.com/mysql-net/MySqlConnector/issues/305).

### 0.24.1

* Recover leaked sessions when `MySqlDataReader` isn't disposed: [#306](https://github.com/mysql-net/MySqlConnector/issues/306).

### 0.24.0

* **Breaking** Add `AllowPublicKeyRetrieval` connection string setting, defaulted to `false`: [#286](https://github.com/mysql-net/MySqlConnector/issues/286).
  * Add `ServerRSAPublicKeyFile` connection string setting.
* Fix hang in `MySqlDataReader.Dispose` if function threw an exception: [#299](https://github.com/mysql-net/MySqlConnector/issues/299).
* Fix `InvalidOperationException` if `MySqlBulkLoader` is used inside a transaction: [#300](https://github.com/mysql-net/MySqlConnector/issues/300).

### 0.23.0

* Support .NET 4.5: [#295](https://github.com/mysql-net/MySqlConnector/issues/295).
* Send client connection attributes: [#293](https://github.com/mysql-net/MySqlConnector/issues/293).
* Dispose `X509Certificate2` objects (.NET 4.6 and later): [#275](https://github.com/mysql-net/MySqlConnector/issues/275).

### 0.22.0

* Add server certificate validation via `MySqlConnectionStringBuilder.CACertificateFile`: [#280](https://github.com/mysql-net/MySqlConnector/pull/280).
* Support `sha256_password` authentication: [#281](https://github.com/mysql-net/MySqlConnector/issues/281).
* Ignore `IOException` in `TryPingAsync`: [#289](https://github.com/mysql-net/MySqlConnector/issues/289).
* Fix "Aborted connection" server errors when `MySqlConnection` isn't disposed: [#290](https://github.com/mysql-net/MySqlConnector/issues/290).
* Run integration tests on MySQL Server 5.6, MySQL Server 5.7, MariaDB 10.3, Percona Server 5.7.

### 0.21.0

* Add `MySqlHelper.EscapeString`: [#277](https://github.com/mysql-net/MySqlConnector/issues/277).

### 0.20.2

* Fix bugs where objects holding unmanaged resources weren't disposed: [#275](https://github.com/mysql-net/MySqlConnector/issues/275).

### 0.20.1

* Fix bug retrieving a connection from the pool when using Amazon IAM Authentication: [#269](https://github.com/mysql-net/MySqlConnector/issues/269).

### 0.20.0

* Support [Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) [IAM Authentication](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html): [#268](https://github.com/mysql-net/MySqlConnector/issues/268).

### 0.19.5

* Fix duration of transaction isolation level: [#263](https://github.com/mysql-net/MySqlConnector/issues/263).
* Fix crash sending a GUID containing `0x27` or `0x5C` when `OldGuids=true`: [#265](https://github.com/mysql-net/MySqlConnector/pull/265).
* Thanks to [Adam Poit](https://github.com/adampoit) for contributions to this release.

### 0.19.4

* Fix `NotImplementedException` in `GetFieldType` and `GetDataTypeName`: [#261](https://github.com/mysql-net/MySqlConnector/issues/261).

### 0.19.3

* Fix authentication against Azure Database for MySQL: [#259](https://github.com/mysql-net/MySqlConnector/issues/259).
* Support enum parameter values: [#255](https://github.com/mysql-net/MySqlConnector/issues/255).
* Fix `CancellationToken` being ignored by `ChangeDatabaseAsync`: [#253](https://github.com/mysql-net/MySqlConnector/pull/253).
* Fix `NullReferenceException` being thrown from `MySqlConnection.CloseDatabase`.
* Thanks to [Nicholas Schell](https://github.com/Nicholi) for contributions to this release.

### 0.19.2

* Fix connection pool exhaustion if connections aren't disposed: [#251](https://github.com/mysql-net/MySqlConnector/issues/251).
* Fix potential `NullReferenceException` in `MySqlDataReader.Dispose`.

### 0.19.1

* Fix incorrect return value from `ExecuteNonQuery`: [#250](https://github.com/mysql-net/MySqlConnector/pull/250).
* Improve performance when retrieving large BLOBs: [#249](https://github.com/mysql-net/MySqlConnector/pull/249).
* Thanks to [Adam Poit](https://github.com/adampoit) for contributions to this release.

### 0.19.0

* Improve performance of common scenarios: [#245](https://github.com/mysql-net/MySqlConnector/pull/245).

### 0.18.3

* Fix query interrupted exception after canceling a completed query: [#248](https://github.com/mysql-net/MySqlConnector/pull/248).
* Thanks to [Adam Poit](https://github.com/adampoit) for contributions to this release.

### 0.18.2

* Fix excessive memory usage with `BufferResultSets=true`: [#244](https://github.com/mysql-net/MySqlConnector/issues/244).

### 0.18.1

* Support new MySQL Server 8.0.1 collations: [#242](https://github.com/mysql-net/MySqlConnector/issues/242).
* Specify preferred collation when resetting connection: [#243](https://github.com/mysql-net/MySqlConnector/issues/243).

### 0.18.0

* Support [`System.Transactions` transaction processing](https://msdn.microsoft.com/en-us/library/ee818755.aspx): [#13](https://github.com/mysql-net/MySqlConnector/issues/13).
* Add `AutoEnlist` connection string option: [#241](https://github.com/mysql-net/MySqlConnector/pull/241).
* Throw better exception for unsupported `ParameterDirection`: [#234](https://github.com/mysql-net/MySqlConnector/issues/234).
* Fix `StackOverflowException` reading a large blob: [#239](https://github.com/mysql-net/MySqlConnector/issues/239).

### 0.17.0

* Implement cancellation of the active reader: [#3](https://github.com/mysql-net/MySqlConnector/issues/3).
* Add `MySqlErrorCode`: [#232](https://github.com/mysql-net/MySqlConnector/issues/232).
* Implement a connection pool reaper to close idle connections: [#217](https://github.com/mysql-net/MySqlConnector/issues/217).
 * Add `ConnectionIdleTimeout` connection string option: [#218](https://github.com/mysql-net/MySqlConnector/issues/218).
* Implement `ConnectionLifeTime` connection string option: [#212](https://github.com/mysql-net/MySqlConnector/issues/212).

### 0.16.2

* Fix exceptions when server resets the connection: [#221](https://github.com/mysql-net/MySqlConnector/issues/221).

### 0.16.1

* Throw a better exception when `max_allowed_packet` is exceeded: [#40](https://github.com/mysql-net/MySqlConnector/issues/40).

### 0.16.0

* Implement `MySqlParameterCollection.AddWithValue`: [#127](https://github.com/mysql-net/MySqlConnector/issues/127).
* Thanks to [michi84o](https://github.com/michi84o) for contributions to this release.

### 0.15.2

* Include help on `AllowUserVariables` in exception message: [#206](https://github.com/mysql-net/MySqlConnector/issues/206).

### 0.15.1

* Fix `NullReferenceException` in `MySqlConnection.Database`: [#205](https://github.com/mysql-net/MySqlConnector/issues/205).

### 0.15.0

* Implement `MySqlConnection.ChangeDatabase`: [#201](https://github.com/mysql-net/MySqlConnector/issues/201).
* Add `Buffer Result Sets` connection string option: [#202](https://github.com/mysql-net/MySqlConnector/issues/202).

### 0.14.1

* Fix exception when `MySqlDataReader` isn't disposed: [#196](https://github.com/mysql-net/MySqlConnector/issues/196).

### 0.14.0

* Update `System.*` package references: [#190](https://github.com/mysql-net/MySqlConnector/issues/190).

### 0.13.0

* Add `MySqlBulkLoader`: [#15](https://github.com/mysql-net/MySqlConnector/issues/15).
* Thanks to [gitsno](https://github.com/michi84o) for contributions to this release.

### 0.12.0

* Add support for `DateTimeOffset`: [#172](https://github.com/mysql-net/MySqlConnector/issues/172), [#175](https://github.com/mysql-net/MySqlConnector/issues/175).
* Thanks to [Sébastien Ros](https://github.com/sebastienros) for contributions to this release.

### 0.11.6

* Fix `PlatformNotSupportedException` on AWS Lambda: [#170](https://github.com/mysql-net/MySqlConnector/issues/170).
* Thanks to [Sebastian](https://github.com/SebastianC) for contributions to this release.

### 0.11.5

* Further improve async and sync performance: [#164](https://github.com/mysql-net/MySqlConnector/issues/164).

### 0.11.4

* No changes in this release.

### 0.11.3

* Improve async performance: [#164](https://github.com/mysql-net/MySqlConnector/issues/164).

### 0.11.2

* Fix InvalidCastException when using aggregate functions: [#54](https://github.com/mysql-net/MySqlConnector/issues/54).

### 0.11.1

* Handle `IOException` in `MySqlSession.DisposeAsync`: [#159](https://github.com/mysql-net/MySqlConnector/issues/159).

### 0.11.0

* Implement the `SslMode=Preferred` connection string option and make it the default: [#158](https://github.com/mysql-net/MySqlConnector/pull/158).

### 0.10.0

* Change minimum supported .NET Framework version to .NET 4.5.1: [#154](https://github.com/mysql-net/MySqlConnector/issues/154).

### 0.9.2

* Fix MySqlConnection.DataSource with Unix Domain Socket: [#152](https://github.com/mysql-net/MySqlConnector/issues/152).

### 0.9.1

* Fix `SocketException` when calling `OpenAsync`: [#150](https://github.com/mysql-net/MySqlConnector/issues/150).

### 0.9.0

* Implement `Treat Tiny As Boolean` connection string option: [#141](https://github.com/mysql-net/MySqlConnector/issues/141).

### 0.8.0

* Implement `Keep Alive` connection string option: [#132](https://github.com/mysql-net/MySqlConnector/issues/132).

### 0.7.4

* Fix `Packet received out-of-order` exception with `UseCompression=true`: [#146](https://github.com/mysql-net/MySqlConnector/issues/146).

### 0.7.3

* Fix `GetDataTypeName` for `ENUM` and `SET` columns: [#52](https://github.com/mysql-net/MySqlConnector/issues/52), [#71](https://github.com/mysql-net/MySqlConnector/issues/71).

### 0.7.2

* Fix authentication for MySQL Server 5.1: [#139](https://github.com/mysql-net/MySqlConnector/issues/139).

### 0.7.1

* Fix `NextResult` incorrectly returning `true`, which may cause problems with Dapper's `QueryMultiple`: [#135](https://github.com/mysql-net/MySqlConnector/issues/135).
* Reduce memory usage related to `Enum.HasFlag`: [#137](https://github.com/mysql-net/MySqlConnector/issues/137).

### 0.7.0

* Implement stored procedure support: [#19](https://github.com/mysql-net/MySqlConnector/issues/19).
 * Known issue: `NextResult` incorrectly returns `true`, which may cause problems with Dapper's `QueryMultiple`: [#135](https://github.com/mysql-net/MySqlConnector/issues/135).

### 0.6.2

* Fix `NullReferenceException` when `MySqlParameter.Value == null`: [#126](https://github.com/mysql-net/MySqlConnector/issues/126).

### 0.6.1

* Fix `AggregateException` going unhandled in `OpenAsync`: [#124](https://github.com/mysql-net/MySqlConnector/issues/124).
* Fix SSL over Unix domain sockets.
* Reduce allocations when using SSL certificates.

### 0.6.0

* Implement `UseCompression` connection string option: [#31](https://github.com/mysql-net/MySqlConnector/issues/31).
* Add support for Unix domain sockets: [#118](https://github.com/mysql-net/MySqlConnector/issues/118).

### 0.5.0

* Implement `UseAffectedRows` connection string option. (Note that the default value is `true`, unlike `MySql.Data`.)

### 0.4.0

* Rename `SslMode` enum to `MySqlSslMode` (for compatibility with `MySql.Data`):[#102](https://github.com/mysql-net/MySqlConnector/pull/102).

### 0.3.0

* Add SSL support and `SslMode` connection string option: [#88](https://github.com/mysql-net/MySqlConnector/issues/88).
* Rewrite protocol serialization layer to support SSL and make adding compression easier: [#93](https://github.com/mysql-net/MySqlConnector/pull/93).

### 0.2.1

* Add more diagnostics for unsupported auth plugins.

### 0.2.0

* Add `MySqlConnectionStringBuilder.ForceSynchronous`: [#91](https://github.com/mysql-net/MySqlConnector/issues/91).
* Thanks to [Ed Ball](https://github.com/ejball) for contributions to this release.

### 0.1.0

* First non-alpha release. Supports core data access scenarios with common ORMs.
