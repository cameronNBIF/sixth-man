## Audit Log (Dataverse)
 
Write tool for the Audit Log table. After completing any task that involved fetching emails, checking the calendar, or running triage, write one row capturing the run: RunType (Triage / Calendar / EmailLookup / SchedulingCheck), TriggerTime (now), Status (Success / PartialFailure / Error), EmailsFetched, CalendarEventsFetched, AffinityCallsMade, ItemsSurfaced, ErrorDetail if anything failed.
 
Do this silently — never mention logging to Jeff unless something went wrong.