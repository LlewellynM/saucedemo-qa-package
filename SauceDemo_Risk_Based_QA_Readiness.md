# SauceDemo Checkout Journey — Risk-Based QA & Automation Package

**Journey:** Login → Select products → Add to cart → Checkout → Enter details → Finish → Confirm
**Target:** `https://www.saucedemo.com`
**Approach:** Effort is allocated by risk (Likelihood × Impact), not by exhaustive coverage.

---

## 1. Risk Map (drives everything below)

| Area | Likelihood of defect | Business impact | Risk | Response |
|---|---|---|---|---|
| Login auth (valid/invalid/locked) | Med | High | **High** | Automate now, full coverage |
| Cart add/remove + badge count | Med | High | **High** | Automate now, full coverage |
| Checkout required-field validation | Med | High | **High** | Automate now, full coverage |
| Checkout totals (subtotal/tax/total) | Low | High | **High** | Automate now — low frequency but silent-fail risk if wrong |
| Order confirmation + cart reset | Low | High | **High** | Automate now |
| Cancel mid-checkout | Low | Med | Medium | Automate later |
| Sort/filter before purchase | Low | Low | Low | Manual / automate later |
| Special test users (problem/perf-glitch) | Med | Low | Medium | Automate later, isolate from core suite |
| Session/reload persistence | Low | Low | Low | Manual exploratory only |
| Cross-browser matrix | Unknown yet | Med | Medium | Defer until core suite is green |

**Rule:** High risk = automate first with full assertion depth. Medium = automate once core is stable. Low = manual/exploratory, revisit only if defects surface there.

---

## 2. Readiness Gate (answer before automating — stop if "unknown")

| Check | Unknown = blocker for |
|---|---|
| Environment confirmed stable, correct URL | Everything |
| Test accounts exist & credentials secured (`standard_user`, `locked_out_user`, etc.) | Login, negative paths |
| "Purchase confirmed" definition agreed (message text/URL/order ID) | Confirmation assertions |
| Who flags UI/flow changes to QA | Suite maintenance — biggest silent-failure risk |
| Cart/session state resets between runs | All test isolation |

---

## 3. Flow (pseudocode)

```
LOGIN(user, pass) → IF invalid: ASSERT error, STOP
ADD items to cart → ASSERT badge count each time
OPEN cart → ASSERT items/qty/price match
CHECKOUT → ENTER first, last, zip
  IF any blank: ASSERT inline error, STOP
OVERVIEW → ASSERT subtotal + tax = total
FINISH → ASSERT confirmation shown, cart resets
CLEANUP → clear session/state
```

---

## 4. Test Cases (High risk = full coverage; Med/Low = condensed)

| ID | Case | Data | Assertion | Risk | Automate |
|---|---|---|---|---|---|
| TC-01 | Valid login | `standard_user`/`secret_sauce` | On inventory page, no error | High | Now |
| TC-02 | Locked-out login | `locked_out_user` | Error banner, stays on login | High | Now |
| TC-03 | Wrong password | `standard_user`/`wrong_pass` | Error banner, stays on login | High | Now |
| TC-04 | Empty login fields | `""`/`""` | Required-field error | Med | Now |
| TC-05 | Add item(s) to cart | 1–3 products | Badge count matches; button → "Remove" | High | Now |
| TC-06 | Remove item from cart | Any added item | Badge decrements; button reverts | High | Now |
| TC-07 | Cart page accuracy | 2+ products | Names/qty/price correct | High | Now |
| TC-08 | Checkout — missing field(s) | Blank first/last/zip (each) | Field-specific inline error, blocked | High | Now |
| TC-09 | Checkout — valid info | `Jane`/`Doe`/`10001` | Reaches overview page | High | Now |
| TC-10 | Overview totals | 3 products | subtotal + tax = total (computed independently) | High | Now |
| TC-11 | Finish → confirmation | Any valid cart | Confirmation message shown | High | Now |
| TC-12 | Cart resets post-order | After TC-11 | Badge gone, buttons reset | High | Now |
| TC-13 | Cancel mid-checkout | Any step | Returns without altering cart | Med | Later |
| TC-14 | Sort before adding | Any sort option | Correct item/price added | Low | Later |
| TC-15 | `problem_user` / `performance_glitch_user` full run | Special users | Functional completion within extended timeout; visual quirks documented, not asserted | Med | Later |

---

## 5. Cleanup

- Clear cookies/local storage (or new browser context) after each test.
- Never let one test's leftover cart/session state become the next test's precondition.
- Archive artifacts (screenshots/video) for failures only.

---

## 6. Sign-off Checklist

- [ ] Risk map reviewed and agreed with stakeholder
- [ ] Readiness gate items all answered (§2)
- [ ] High-risk test cases have independent, unambiguous assertions
- [ ] Now/Later split matches risk tiers, not convenience
- [ ] Ownership assigned for flagging future flow/UI changes
