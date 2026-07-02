This is a Prompt Builder prompt that gets registered as a tool. Create it in Copilot Studio via: Tools → Add a tool → New tool → Prompt.

---

## Tool name (registered name)

CalendarSummary

## Tool description (paste into the Description field)

Formats a list of fetched calendar events into a clean, mobile-readable summary with attention flags. Call this prompt after fetching events from Outlook calendar. Returns events in chronological order with inline flags for external attendees, missing agendas, imminent meetings, and back-to-back blocks. Use whenever the user asks about his calendar, schedule, or what's coming up.

## Input variables to define

- events (text): list of calendar events fetched from Outlook (start time, end time, title, description/agenda if any, attendees with emails)
- queryWindow (text): the time window the user asked about (e.g. "today", "tomorrow", "this week", "Friday morning")
- emailHistory (text, optional): brief summary of prior email exchanges with external attendees, if available — used to flag never-met attendees

## Prompt body (paste into the Prompt field)

You are formatting the user's calendar summary. You have been given:

EVENTS:
{events}

QUERY WINDOW:
{queryWindow}

EMAIL HISTORY (may be empty):
{emailHistory}

Your job: produce a clean, mobile-readable summary with attention flags inline.

## Output format

For each event, output a single line in this format:
[Time in AST/ADT] — [Title] — [Duration] — [Attendees]

All times must be in Atlantic Time. If the source data is in UTC, convert before presenting.

## Inline flags

Apply these flags directly on the event's line:

- Any attendee whose email domain is NOT @nbif.ca → add "(external)" next to their name.
- Event has no description, no agenda, or only a calendar invite stub with no content → append "(no agenda — consider requesting one)" at the end of the line.
- Event starts within the next 60 minutes from now → prefix the entire line with "⚠️ STARTING SOON".
- If email history is provided and an external attendee has no prior email exchange with the user → append "(no prior contact)" beside their name.

## Block flags

After the event list, check for back-to-back blocks: any sequence of meetings running consecutively for more than 90 minutes with no gap between them. If found, add a line:
"⚠️ You have a [X]-hour block with no break from [start time] to [end time]."

There can be multiple such blocks in one summary — flag each one separately.

## Presentation rules

1. Events in strict chronological order.
2. One line per event, no multi-line entries.
3. Keep the entire response readable on a mobile phone screen.
4. If the events list is empty, return: "Nothing scheduled in [query window]."

## Summary line

End with exactly one summary line in this format:
"X meetings [in window]. Y external. Z need prep or have no agenda."

Where:
- X is total event count
- Y is count of events with at least one external attendee
- Z is count of events flagged "no agenda"

Output nothing other than the event lines, any back-to-back block flags, and the final summary line. No preamble, no postamble.