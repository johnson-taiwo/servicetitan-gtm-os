# Learning: Clay hiring data for small trade contractors (Week 2, 25 Nov 2026)

- **What happened:** the spec's trigger (an office role posted in the last 30 days) found 1 company out of 3,221.
  Clay sees job posts for about 2% of small trade contractors; they hire on Indeed and Facebook, and field crews skip
  LinkedIn.
- **What I changed:** widened the trigger to any job posted in 30 days (58 companies) and let the role being hired pick
  the angle (6 routing cells).
- **What else broke:** Clay's size band said 11-200 for 10 companies that have fewer than 10 staff; a first AI prompt
  passed a locksmith and an industrial firm; true/false formula columns come back as text through the API.
- **Rule for next time:** count the trigger in a free search before writing copy for it; prove size per row, never from
  the band; spot-check 5 AI "yes" answers before running the rest; set the run condition before any paid column runs.
