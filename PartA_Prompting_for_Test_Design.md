# Part A — Prompting for Test Design

## The Prompt

> Design test cases for this checkout feature: "As a customer, I want to update my delivery address during checkout so my order is delivered correctly."
>
> Known constraints: feature is "mostly done" but validations aren't finalized; postcode rules may vary by country but this is **unconfirmed**; existing flow supports saved addresses, guest checkout, and promo codes — these must not regress; some users have multiple saved addresses; order management and confirmation email both consume the address post-checkout; test data is limited/shared; no RTM or acceptance criteria yet.
>
> Give me: scenarios grouped by risk (high/med/low) covering happy path, negative/validation cases, edge cases (multi-address, guest checkout, promo persistence), and integration points (order system, email). For each: preconditions, steps, data, expected result. **Flag every assumption you make where requirements are missing** — don't invent business rules. Flag any case needing test data that may not exist in a limited pool.

**Why it's built this way:** it separates known facts from open questions so the model can't blend guesses with requirements, and forces it to surface assumptions instead of presenting them as settled.

---

## How I'd Validate the AI's Output

- Trace every case back to the story or a flagged assumption — unlinked cases get dropped.
- Reject invented rules (e.g., a specific postcode format) not confirmed by the BA.
- Confirm integration points (order system, email) are actually covered, not just the UI.
- Check for padded/duplicate cases inflating apparent coverage.
- Verify test data for edge cases is realistically available given the limited/shared pool.
- Nothing is treated as final until a human signs off — especially anything built on an unconfirmed requirement.

## What I'd Never Trust Without Manual Review

- Specific validation rules (postcode format, required fields) — must come from the BA, not be inferred by the model.
- Claims of "complete coverage" — meaningless against an unfinished spec, and models tend to overstate this.
- Priority/severity ratings — these need business judgment (cost of a misdelivered order vs. a cosmetic bug), not a model's guess.
- Assumptions about how downstream systems (order management, confirmation email) actually consume the address — a wrong assumption here could hide a real integration defect.
