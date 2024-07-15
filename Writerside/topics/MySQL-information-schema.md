# MySQL-information_schema

### get last update of database return null (mysql)

> **Highlight important information**
>
> Beginning with MySQL 5.7.2, UPDATE_TIME displays a timestamp value for the last UPDATE, INSERT,
> or DELETE performed on InnoDB tables that are not partitioned. Previously, 
> UPDATE_TIME displayed a NULL value for InnoDB tables. For MVCC, the timestamp value reflects the COMMIT time, 
> which is considered the last update time.
> Timestamps are not persisted when the server is restarted or 
> when the table is evicted from the InnoDB data dictionary cache.
>
> The UPDATE_TIME column also shows this information for partitioned InnoDB tables in MySQL 5.7.8 and later. 
> Previously this column was always NULL for such tables. (Bug #17299181, Bug #69990)
{style="note"}
