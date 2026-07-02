This is a Prompt Builder prompt that gets registered as a tool. Create it in Copilot Studio via: Tools → Add a tool → New tool → Prompt.

---

## Tool name (registered name)

TriageEmail

## Tool description (paste into the Description field)

Formats a list of fetched emails into prioritized triage cards. Call this prompt after fetching emails from Outlook and looking up their senders in VIP Contacts and Affinity. Returns Tier 1 CRITICAL and Tier 2 IMPORTANT items as structured cards, filters out lower-tier items, and ends with a count summary. Use whenever the user asks to triage or prioritize his inbox.

## Input variables to define

- emails (text): a list of emails fetched from Outlook (sender name, sender email, subject, brief preview/snippet, received timestamp, has-deadline flag if known)
- vipLookups (text): VIP Contacts results for each sender (email → tier and category, or "not found")
- affinityLookups (text): Affinity relationship results for Tier 1/2 senders (relationship strength, last interaction date in days), if available

## Prompt body (paste into the Prompt field)

You are formatting the user's email triage. You have been given three pieces of data:

EMAILS:
{emails}

VIP LOOKUPS:
{vipLookups}

AFFINITY LOOKUPS (may be empty if Affinity wasn't called or returned no matches):
{affinityLookups}

Your job: assign each email a triage tier, then output structured cards for the important ones.

## Tier assignment rules

- **Tier 1 CRITICAL**: sender is a VIP at Tier 1 (per VIP Lookups), OR the email contains an explicit deadline today (look for phrases like "by EOD," "by end of day," "today," "needs answer today" in the subject or preview).
- **Tier 2 IMPORTANT**: sender is a VIP at Tier 2, OR the email is a substantive request (decision needed, document review, meeting proposal) from a known partner or portfolio company even if not on the VIP list.
- **Tier 3 FYI**: informational only — CCs where the user isn't the primary, newsletters from relevant sources, status updates.
- **Tier 4 NOISE**: cold outreach, automated notifications, marketing, unsubscribe-able lists.

A Tier 1 VIP email is always Tier 1 CRITICAL regardless of subject matter.

## Output format

For each Tier 1 and Tier 2 email only, output a card in this exact format:

**[TIER 1 CRITICAL]** or **[TIER 2 IMPORTANT]**
- Who: [sender name] | [organization, from VIP category or inferred from email domain]
- Subject: [subject line]
- Relationship: [relationship strength and days since last contact, ONLY if Affinity returned a match for this sender; omit this line entirely otherwise]
- Situation: [2 sentences max — what the email is actually about, in your own words, no verbatim quoting]
- the user needs to: [one clear, specific action — never "review this"]
- Options: [2–3 concrete next steps the user can choose from, comma-separated]

## Presentation rules

1. Present all Tier 1 items first, then Tier 2.
2. Do not mention Tier 3 or Tier 4 items in the output at all — they're filtered out silently.
3. Never paste full email bodies — summarize only, 2–3 sentences max per email.
4. Never quote the original email verbatim beyond a short attributed phrase like the subject line.
5. Keep the entire response readable on a mobile phone screen.
6. End with exactly one summary line in this format: "X critical, Y important — Z total emails reviewed."

If the emails list is empty, return: "No emails to triage in the requested window."

If you cannot determine the sender or subject for a particular email due to missing data, skip it silently rather than fabricating details.

Output nothing other than the triage cards and final summary line. No preamble, no postamble.