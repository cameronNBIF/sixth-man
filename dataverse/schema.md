# Table 1: VIP Contacts

This is the triage backbone — the agent checks every sender against this list.

| Column | Type | Notes |
|--------|------|-------|
| Name | Text (Primary) | Full name |
| Email | Text | Exact email address |
| Organization | Text | Company or org name |
| Tier | Choice | 1 = Board/Investors, 2 = Government/Key Partners, 3 = Portfolio/Internal |
| Category | Choice | Board, Government, Portfolio, LP, Internal, Other |
| Notes | Multiline Text | e.g. "always flag even if FYI topic" |
| IsActive | Yes/No | Default: Yes |

**Seed this table immediately with ~20–30 contacts from a 30-minute conversation with Jeff. The agent is useless without it.**

---

# Table 2: Jeff's Preferences

| Column | Type | Notes |
|--------|------|-------|
| PreferenceKey | Text (Primary) | e.g. no_meetings_before, vip_override_sarah |
| PreferenceValue | Multiline Text | e.g. 9:00 AM, always escalate regardless of topic |
| LearnedDate | Date | When the preference was captured |
| Source | Choice | Explicit (Jeff told it), Observed (inferred from behavior) |
| Confirmed | Yes/No | Has Jeff confirmed this preference? |

---

# Table 3: Audit Log

| Column | Type | Notes |
|--------|------|-------|
| RunID | Text (Primary) | GUID per agent run |
| RunType | Choice | MorningBrief, MeetingBrief, FridayPlan, AdHoc |
| TriggerTime | Date/Time | When the run fired |
| Status | Choice | Success, PartialFailure, Error |
| EmailsFetched | Number | Count only, not content |
| CalendarEventsFetched | Number | Count |
| AffinityCallsMade | Number | Count |
| ItemsSurfaced | Number | How many cards/items went to Jeff |
| ErrorDetail | Multiline Text | Exception message if failed |

---

# Table 4: Briefed Events

| Column | Type | Notes |
|--------|------|-------|
| EventID | Text (Primary) | Graph event ID |
| EventStartTime | Date/Time | Composite dedup key with EventID |
| BriefSentAt | Date/Time | When the pre-meeting brief fired |
| BriefType | Choice | PreMeeting, MorningBrief |