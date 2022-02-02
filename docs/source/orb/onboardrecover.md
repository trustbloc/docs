# Onboarding and Recovery

Onboarding and recovery of a domain are closely related. Onboarding takes place when a new domain comes online.
This means that it starts to [follow](activitypub.html#follow) other domains, asks other
domains to be its [witness](activitypub.html#invite-witness), and potentially other domains
will want to follow it and ask it to be a witness. Recovery is the process that takes place
after a domain has been offline for a while and needs to catch up with the activities
posted by the domains it's following while it was down. Recovery could also mean that a
domain's database had to be restored from a backup and it needs to recover
data that was lost and also catch up with the missed activities from the domains that it is
following.

## Activity Sync Task

When an Orb server starts up, an _activity sync_ task is registered with the
[Task Manager](taskmanager.html#task-manager). This task [periodically](parameters.html#anchor-event-sync-interval)
synchronizes anchor activities ([Create](https://trustbloc.github.io/activityanchors/#create-activity)
and [Announce](https://trustbloc.github.io/activityanchors/#announce-activity)) with the
domains that the local domain is following and also processes any missing anchor
activities ([Creates](https://trustbloc.github.io/activityanchors/#create-activity)
that were posted to the domains followers).

The _activity sync_ task is run on one server instance in a domain. It first reads from the
[Inbox](restendpoints/activitypub.html#inbox) of each of its followers to
ensure that the Create activities that it had posted to its followers are actually stored locally.
If not, it pulls the data from its followers and [processes](activitypub.html#create-announce)
the anchor event. The task then reads the [Outbox](restendpoints/activitypub.html#outbox)
of the domains that it is following and [processes](activitypub.html#create-announce) all Create and
Announce activities.

When processing an activity, the timestamp of when the activity was published is used to determine
the _age_ of the activity. The activity is not processed unless its age has reached the configured
[minimum activity age](parameters.html#anchor-event-sync-min-activity-age). This is done to minimize
the possibility of both the [Inbox](activitypub.html#outbox-inbox) and the activity sync task concurrently
processing the anchor event. (Even though the system is tolerant of this situation, it still has an impact
on performance.)

Once processing has finished, the _activity-sync_ database is updated with the last page and index
of each domain that it processed so that when the task runs again, processing starts from where
it left off.

```{image} ../_static/orb/ap-activity-sync.svg

```
