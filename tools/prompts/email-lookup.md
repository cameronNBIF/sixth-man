This is a Prompt Builder prompt that gets registered as a tool. Create it in Copilot Studio via: Tools → Add a tool → New tool → Prompt.

---

## Tool name (registered name)

EmailLookup

## Tool description (paste into the Description field)

Produces a structured summary of a specific email the user has asked about by sender name or subject. Call this prompt after fetching candidate emails from Outlook and looking up sender context in VIP Contacts and Affinity. Returns a Context / Situation / Ask / Options summary with relationship intelligence, identifying the single most relevant email if multiple matches were found. Use whenever the user asks to summarize, look up, or recap a specific email or thread.

## Input variables to define

- candidates (text): list of email candidates fetched from Outlook matching the user's search (sender name, sender email, subject, received timestamp, body preview, thread message count if known)
- searchQuery (text): what the user actually asked for (e.g. "the email from Tony Sheehan", "the thread about the Q3 funding round")
- vipLookup (text): VIP Contacts result for the sender of the most relevant candidate (tier, category)
- affinityLookup (text, optional): Affinity result for the sender (relationship strength, last interaction date in days)

## Prompt body (paste into the Prompt field)

You are summarizing a specific email for the user. You have been given:

CANDIDATES:
{candidates}

SEARCH QUERY:
{searchQuery}

VIP LOOKUP:
{vipLookup}

AFFINITY LOOKUP (may be empty):
{affinityLookup}

## Selection logic

If there are multiple candidates, pick the single most relevant one based on:
1. Direct match to the search query (sender name, subject keywords)
2. Recency — more recent wins ties
3. If still tied, prefer the email with the most thread context (longest thread)

If none of the candidates are a clear match to the search query, return:
"Couldn't find an email matching '[searchQuery]'. Closest matches were: [list 1-3 candidates with sender and subject]. Want me to look at a specific one?"

If candidates list is empty, return:
"No emails found matching '[searchQuery]'. Want to try a different sender or subject?"

## Output format

For the selected email, output exactly this structure:

**From:** [sender name] | [organization, from VIP category or email domain] | [date received in Atlantic Time]
**Subject:** [subject line]

**Context:** [Who this person is and why they matter to the user. 1–2 sentences. Use VIP tier (e.g. "Tier 1 Board Member") and Affinity relationship context (e.g. "strong relationship, last contact 47 days ago") if available. If neither lookup returned data, describe based on email domain or signature.]

**Situation:** [What the email is actually about. 2–3 sentences. Be specific — include any numbers, dates, decisions, or asks mentioned. Never quote verbatim.]

**The user needs to:** [One clear, specific action. Not "review this" — name the actual decision or response needed.]

**Options:**
- [Option 1 — a concrete next step, e.g. "Reply confirming attendance by Friday"]
- [Option 2 — alternative, e.g. "Forward to Sarah and ask her to draft a response"]
- [Option 3, optional — e.g. "Request a 15-min call before responding"]

## Thread handling

If the selected email is part of a thread with more than one message, add this line ABOVE the structured summary:
"[N]-message thread. Prior context: [one sentence summarizing what came before this latest message]."

## Rules

1. Never quote the email body verbatim. Summarize and interpret only.
2. Subject line CAN be quoted directly.
3. Keep the response readable on a mobile phone screen.
4. If VIP Lookup says the sender is flagged as board/legal/HR correspondence, still produce the summary — but add a single line at the top: "⚠️ Sensitive: board/legal/HR — discuss only in this conversation."

Output nothing other than the structured summary (with optional thread/sensitivity prefix lines). No preamble, no postamble, no "Here's the summary:".