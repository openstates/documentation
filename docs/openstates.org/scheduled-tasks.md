# Openstates.org scheduled tasks

Openstates.org is a django application that has some simple scheduled tasks that execute using [Django admin commands](https://docs.djangoproject.com/en/4.0/howto/custom-management-commands/).

## Jobs and Locations

Containerized versions of the actual scheduled jobs are managed in [Github](https://github.com/openstates/openstates.org/tree/develop/docker/cron). These are _not_ the jobs executed in production.

We deploy jobs to the Openstates.org host using [ansible](https://github.com/openstates/openstates.org/blob/develop/ansible/openstates/tasks/main.yml#L70).

openstates.org runs in AWS. Access credentials/location/etc. can be found in the AWS console.

## Currently Scheduled Jobs

### Subscription Processing

[Defined here](https://github.com/openstates/openstates.org/blob/develop/profiles/management/commands/process_subscriptions.py)

Tool that processes search subscriptions for users.

Currently (2022-08-02) scheduled to run once a day (12:30 UTC)

### Aggregate API Usage

[Defined here](https://github.com/openstates/openstates.org/blob/develop/profiles/management/commands/aggregate_api_usage.py)

Tool that generates some internal stats for user interactions with Openstates.

Currently (2022-08-02) scheduled to run every 2 hours (39 */2)

### System Maintenance Jobs

* Let's Encrypt certificate collection/rotation
* Nginx maintenance (tied to certificate rotation)
