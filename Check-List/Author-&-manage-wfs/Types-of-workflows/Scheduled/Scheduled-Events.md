# Configure different events for Workflows to run

## Scheduled Events
---- 
- Use the `schedule` keyword
- Can be delayed during periods of high loads of GH actions workflow runs
- If load is high enough, some queued jobs may be dropped
- To decrease chance of delay, schedule wfs to run at different times
- Event will only trigger a wf run if the wf file exists on the default branch
- In a public repo, wf are automatically disabled when no repo activity occurred in 60 days


Example:
```
on:
   schedule:
     - cron: "15 4,5 * * *"
```
- Use POSIX cron syntax to schedule wfs to run at specific times
- Run in UTC by default
- Timezones can be specified with IANA timezone string
- Run on the latest commit on the default branch
- Shortest time interval ➡️ once every 5 minutes
- Single wf can be triggered by multiple `schedule` events
- Event that triggered wf ➡️ `github.event.schedule` context

### Cron Syntax
```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12 or JAN-DEC)
│ │ │ │ ┌───────────── day of the week (0 - 6 or SUN-SAT)
│ │ │ │ │
* * * * *
```

|Operator|Description|Example|
|--------|-----------|-------|
|   *    |Any Value           |`15 * * * *` runs at every minute 15 of every hour of every day.|
|   ,    |Value list separator|	`2,10 4,5 * * *` runs at minute 2 and 10 of the 4th and 5th hour of every day.|
|   -    |Range of values     |`30 4-6 * * *` runs at minute 30 of the 4th, 5th, and 6th hour.|
|   /    |Step values         |`20/15 * * * *` runs every 15 minutes starting from minute 20 through 59 (minutes 20, 35, and 50).|

### `actor` for scheduled workflows
- Certain repository events change the `actor` associated with the workflow.
    - User who changes the default branch of the repo, which changes the branch on which scheduled workflows run ➡️ becomes `actor` for those  scheduled workflows
    - Deactivated scheduled workflow ➡️ user with `write` permissions changes the `cron` schedule ➡️ workflow gets reactivated ➡️ user becomes the `actor` associated with any workflow runs
    - **Notifications** - last user who modified the `cron` syntax will be sent notifications

#### Enterprise with Enterprise Managed Users
- Status of `actor` user account associated with the wf is active (not deleted or suspended)
- Managed user removed from a organization but **NOT** deprovisioned ➡️ still the `actor`
- Managed user deprovisioned by IdP (Enterprise Managed User identity provider) ➡️ no longer  the `actor`

#### Enterprise without Enterprise Managed Users
- Removing user from a organization ➡️ still the `actor`

**The user account status is more important than their membership status**