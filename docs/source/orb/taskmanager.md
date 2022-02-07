# Task Manager

The Task Manager is an Orb service that periodically runs tasks on one Orb instance
in the domain. A task is registered on startup with a unique ID, a run interval,
and a function to invoke at the registered interval. The Task Manager stores a _permit_ for
each task in the _orb-config_ database. The permit contains:
- **Task ID**: The unique ID of the task
- **Permit Holder**: The unique ID of the Orb instance that is currently responsible for running the task (a GUID that's generated on startup)
- **Status**: Either _idle_ or _running_
- **Update Time**: The last time the permit was updated (used to check the last time the task was run and the _aliveness_ of the permit holder)

The Task Manager on each Orb instance [periodically](parameters.html#task-manager-check-interval)
checks the permit for each task. Different actions are taken depending on whether the Orb instance
holds the permit, as described below.

```{image} ../_static/orb/task-manager.svg

```

### This Orb instance holds the permit

When it is determined that this instance is the permit holder for the task, the permit is checked for
its status: either _idle_ or _running_.

#### Idle status

If the status of the task is _idle_ then a check is made to see if it is time to run the task
using the registered interval for the task and the last update time.
If it is time to run the task then:

1) The status of the permit is set to _running_
2) The last update time is set to the current time 
3) The task is started

#### Running status

If the status of the task is _running_ then the permit is updated with the current time so that
other instances still see that this instance is alive, otherwise (for long-running tasks)
other instances may think that this instance is down and attempt to acquire the permit.

### Another Orb instance holds the permit

When it is determined that this instance is NOT the permit holder for the task, the health of the
permit holder is checked. If the time since the permit was updated is greater than the Task Manager's
[check interval](parameters.html#task-manager-check-interval) plus the interval of the task itself,
then it is assumed that the permit holder is down and this instance attempts to take over the permit.
There is a window in which multiple instances may attempt to take over the permit at the same
time, in which case multiple instances may run the task concurrently. This should only happen once
during a _takeover_ event, since the last instance to update the permit is responsible for all future
runs.

## Tasks

Tasks registered with the Task Manager must be tolerant of the fact that multiple server instances
may run the task concurrently. This may happen when a permit holder goes down and
other instances attempt to take over the permit (as described in the section above). Following is a
description of the registered tasks:

### Database Expiry

The database expiry task periodically deletes expired data from multiple databases. The database expiry
service allows multiple databases to be registered for expired data checks. A database is registered
with a database name and a tag that contains the expiration time of the document. The expiry task queries
for all documents from the registered databases where the expiry time has been reached, and
deletes these documents. The scheduled run period is set with the startup
parameter [data-expiry-check-interval](parameters.html#data-expiry-check-interval).

### Activity Sync

The [Activity Sync](onboardrecover.html#activity-sync-task) task periodically synchronizes anchor activities from the
inboxes and outboxes of its followers and the domains that it's following. The scheduled period is set
with the startup parameter [sync-interval](parameters.html#anchor-event-sync-interval).

### Anchor Status Monitor

This task monitors the status of an anchor event that is waiting for proofs from witnesses.
The task checks if enough proofs have been received for an anchor event and, if not, a new
set of witnesses is solicited for proofs. The scheduled period for this task is set with
parameter, [anchor-status-monitoring-interval](parameters.html#anchor-status-monitoring-interval).

### VCT Monitor

The [VCT](vct.html) monitor task checks if a proof that was returned by a witness
(in an [Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity)
activity) was actually added to the witness's ledger. The scheduled period
for this task is set with parameter,
[vct-monitoring-interval](parameters.html#vct-monitoring-interval).

### Operation Queue Monitor

This [task](operationqueue.html#operation-queue-monitor-task) monitors the operation queue to ensure that if a server
instance goes down then another instance processes the queued operations. The scheduled period is set
with parameter, [task-manager-check-interval](parameters.html#task-manager-check-interval).
