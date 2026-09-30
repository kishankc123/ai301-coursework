Hi! I'd like to work on this as a course exercise.

I'll reproduce the two behaviors the issue describes on a clean checkout of main: scrub() leaving (555) 123-4567 unredacted while it redacts 555-123-4567 in the same string, and detect() returning [] for the parenthesized format, plus the four named tests (test_us_phone_number_redaction, test_us_phone_formats, test_detect_phone_pii, test_phone_at_start_of_text) in tests/unit/test_pii_scrubber.py. I'll post a reproduction report here with my environment, the exact commands I ran, and what I observed, before looking at any fix.
