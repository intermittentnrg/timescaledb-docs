---
title: Upgrade PostgreSQL
excerpt: Upgrade the PostgreSQL installation associated with TimescaleDB
products: [self_hosted]
keywords: [upgrades, PostgreSQL, versions, compatibility]
---

import ConsiderCloud from "versionContent/_partials/_consider-cloud.mdx";

# Upgrade PostgreSQL

Because TimescaleDB is a PostgreSQL extension, you need to ensure you keep your
underlying PostgreSQL installation up to date. When you upgrade your TimescaleDB
extension to a new version, always take the time to check if you need to also
upgrade your PostgreSQL version. If you are running an older version of
PostgreSQL, you need to upgrade it first, before you upgrade your TimescaleDB
extension.

<ConsiderCloud />

TimescaleDB supports these PostgreSQL releases. If you are not running a
compatible PostgreSQL version, make sure you upgrade PostgreSQL before you
upgrade TimescaleDB:

||PostgreSQL&nbsp;17|PostgreSQL&nbsp;16|PostgreSQL&nbsp;15|PostgreSQL&nbsp;14|PostgreSQL&nbsp;13|PostgreSQL&nbsp;12|PostgreSQL&nbsp;11|PostgreSQL&nbsp;10|PostgreSQL&nbsp;9.6|
|-|-|-|-|-|-|-|-|-|
|TimescaleDB&nbsp;2.17 and higher|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.16 and higher|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.15 and higher|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.14 and higher|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.13 and higher|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.12 and higher|&#10060;|&#10060;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.10 and higher|&#10060;|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.5 to 2.9|&#10060;|&#10060;|&#10060;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.4|&#10060;|&#10060;|&#10060;|&#10060;|&#9989;|&#9989;|&#10060;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.1 to 2.3|&#10060;|&#10060;|&#10060;|&#10060;|&#9989;|&#9989;|&#9989;|&#10060;|&#10060;|
|TimescaleDB&nbsp;2.0|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|&#9989;|&#9989;|&#10060;|&#10060;
|TimescaleDB&nbsp;1.7|&#10060;|&#10060;|&#10060;|&#10060;|&#10060;|&#9989;|&#9989;|&#9989;|&#9989;|

We recommend not using TimescaleDB with PostgreSQL 17.1, 16.5, 15.9, 14.14, 13.17, 12.21.  
These minor versions [introduced a breaking binary interface change][postgres-breaking-change] that,
once identified, was reverted in subsequent minor PostgreSQL versions 17.2, 16.6, 15.10, 14.15, 13.18, and 12.22.
When you build from source, best practice is to build with PostgreSQL 17.2, 16.6, etc and higher.
Users of [Timescale Cloud](https://console.cloud.timescale.com/) and platform packages for Linux, Windows, MacOS,
Docker, and Kubernetes are unaffected.

You cannot upgrade TimescaleDB and PostgreSQL at the same time. You upgrade each product in
the following steps: 

1. Upgrade TimescaleDB to the desired version in your current PostgreSQL installation. 
2. Upgrade PostgreSQL to the desired version. 
 
The version of TimescaleDB installed in your PostgreSQL deployment must be the same before 
and after the PostgreSQL upgrade.

## Plan your upgrade

You can upgrade your PostgreSQL installation in-place. This means
that you do not need to dump and restore your data. However, it is still
important that you plan for your upgrade ahead of time.

Before you upgrade:

*   Read [the release notes][pg-relnotes] for the PostgreSQL version you are
  upgrading to.
*   [Perform a backup][backup] of your database. While PostgreSQL and
  TimescaleDB upgrades are performed in-place, upgrading is an intrusive
  operation. Always make sure you have a backup on hand, and that the backup is
  readable in the case of disaster.

## Upgrade PostgreSQL

You can use the [`pg_upgrade`][pg_upgrade] tool to upgrade PostgreSQL in-place.
This means that you do not need to dump and restore your data. Instead,
`pg_upgrade` allows you to retain the data files of your current PostgreSQL
installation while binding the new PostgreSQL binary runtime to them. This is
supported for PostgreSQL&nbsp;8.4 and higher.

<Procedure>

### Upgrading PostgreSQL

1.  Before you begin, determine the location of the PostgreSQL binary and your
    data directory on your local system.
1.  At the psql prompt, perform the upgrade:

    ```sql
    pg_upgrade -b <OLD_BIN_DIR> -B <NEW_BIN_DIR> -d <OLD_DATA_DIR> -D <NEW_DATA_DIR>
    ```

</Procedure>

If you are moving data to a new physical instance of PostgreSQL, you can use the
`pg_dump` and `pg_restore` tools to dump your data from the old database, and
then restore it into the new, upgraded, database. For more information, see the [backup and restore section][backup].

[backup]: /self-hosted/:currentVersion:/backup-and-restore/
[pg-relnotes]: https://www.postgresql.org/docs/release/
[pg_upgrade]: https://www.postgresql.org/docs/current/static/pgupgrade.html
[postgres-breaking-change]: https://www.postgresql.org/about/news/postgresql-172-166-1510-1415-1318-and-1222-released-2965/
