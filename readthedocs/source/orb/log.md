# Log configuration

An administrator can configure log per domain. When configured the log URL
is stored into configuration database "orb-config" under "log-url" key.
The log URL is cached on the server with periodic cache updates from the database.
Default log URL cache expiry period has been set to 1 minute.

### Configuring log URL

Log URL can be configured by posting log URL to domain endpoint "/policy".

### Retrieve log URL

Log URL can be retrieved by issuing GET to domain endpoint "/policy".

# Log Monitoring

Orb server will periodically invoke log monitoring service that will watch logs for consistency 
and check that they behave correctly.
In order to watch logs, the monitoring service follows these steps for each log:
- Fetch the current signed tree head(STH).
- Verify the STH signature.

If the service is processing log for the first time and the log is not empty:
- Fetch all the entries in the tree corresponding to the STH. 
- Confirm that the tree made from the fetched entries produces the same hash as that in the STH. 

If the service has already encountered this log and log's STH has changed since last check:
- Fetch a consistency proof for the new STH with the previous STH.
- Verify the consistency proof.

Upon successful STH signature and consistency verification, log monitoring service will 
save current STH for each domain.

The service can be configured to retrieve and store all log entries during log monitoring. See [Log Entries Store Enabled Parameter](parameters.html###vct-log-entries-store-enabled).

For more details about log monitoring see: https://datatracker.ietf.org/doc/html/rfc6962#section-5.3.