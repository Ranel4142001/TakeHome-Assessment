Notes

1. One fix explained — the double-booking bug

The code that checks for overlapping bookings only looked at whether the
start date of the new booking fell inside the old booking's dates. It
never checked the end date. So if a new booking started earlier than an
existing one but still crossed into it, the code missed it and let the
booking through.

The fix: check both bookings properly. Two date ranges overlap unless one of
them completely ends before the other one begins. Here's the fixed line:

(return start_a <= end_b and start_b <= end_a)

The one line of code above with parenthesis covers every case — a booking starting early,
one ending late, one fully inside another, or an exact match.



2. Show the failure (Task 1)

There's already a booking for the Canon DSLR Camera from 2026-01-10 to
2026-01-15.

Before the fix, the app would still let someone book the same camera for
2026-01-05 to 2026-01-12, even though those dates clearly overlap
(Jan 10–12 is booked twice). After the fix, this booking is correctly
blocked.



3. AI use

I used Claude to help me understand the code, find where each bug was
coming from, and explain why each fix works.

To check the fixes were actually correct, I:


Ran the app myself and tried the bookings in the browser (for example,
the Jan 5–12 example above, both before and after the fix)
Watched what the server printed in the terminal for each request
Counted days on a calendar by hand and calculate to check the prices were right
Read every code change myself before adding it, instead of just
copy-pasting without checking