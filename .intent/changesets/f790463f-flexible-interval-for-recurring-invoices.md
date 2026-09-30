# Flexible Interval for Recurring Invoices

## Summary

Recurring invoices currently always repeat with a fixed step of exactly 1 unit (every day, every week, every month, every year). Users cannot configure intervals like "every 3 months" or "every 5 weeks".

This changeset adds a configurable **interval** field to the recurring invoice schedule. Combined with the existing recurrence type (daily / weekly / monthly / yearly), users can define any regular cadence. The default interval of 1 preserves full backward compatibility with existing invoices.

## Acceptance Criteria

- A numeric input "Repeat every [N] [type]" is visible in the recurring invoice schedule form for all four recurrence types (daily, weekly, monthly, yearly).
- The interval field accepts whole numbers ≥ 1 only; the default value is 1.
- Saving a recurring invoice with interval N causes invoices to be generated every N units of the selected type (e.g. every 3 months).
- The "days" selection (weekdays, days of month, months of year) continues to work as before within each interval window.
- The frequency label throughout the UI (invoice list, detail view, occurrences preview) reflects the interval — e.g. "Every 3 months on the 1st".
- End-date and "after X occurrences" calculations correctly account for the interval (e.g. monthly × interval 3 × 4 occurrences = 12 months total).
- Existing recurring invoices are unaffected and behave exactly as before (implicit interval of 1).
- The form validation shows an error if the interval is set to 0 or left empty.

## Testing Notes

- Create a recurring invoice with type **Monthly**, interval **3**, day **1** — verify the next occurrences are Jan 1 → Apr 1 → Jul 1 → Oct 1.
- Create a recurring invoice with type **Weekly**, interval **2**, day **Monday** — verify occurrences are 2 weeks apart.
- Edit an existing recurring invoice (without touching the interval) — confirm it defaults to interval 1 and schedule is unchanged.
- Verify the end-date calculation for "after 4 occurrences" with monthly × interval 3 returns a date 12 months from start.
- Test interval 1 for all four types produces the same dates as today (regression).

## Key Decisions

- Interval defaults to 1, ensuring no migration is needed for existing data.
- No shortcut buttons for "quarterly" or "semi-annual" — users set interval 3 or 6 with Monthly type, keeping the model simple.


## Specs

- [Flexible Interval for Recurring Invoices](../specs/48472781.md)

---

[View in Intent](https://app.onintent.build/?project=8c4299ba-f9fc-474c-a0fb-4fa1d53b0a82&changeset=f790463f-44e8-4e79-94c8-1fd83f1d195b&tab=detail)