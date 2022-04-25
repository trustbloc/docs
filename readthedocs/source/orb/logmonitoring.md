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

Upon successfull STH signature and consistency verificiation, log monitoring service will 
save current STH for each domain.

The service can be configured to retrieve and store all log entries during log monitoring. See [Log Entries Store Enabled Parameter](parameters.html###vct-log-entries-store-enabled).

For more details about log monitoring see: https://datatracker.ietf.org/doc/html/rfc6962#section-5.3.