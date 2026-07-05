# SauceDemo Checkout — Risk-Based QA & Automation Package

A compact, risk-based QA design and automation-readiness package for the SauceDemo checkout journey: **Login → Select products → Add to cart → Checkout → Enter details → Finish → Confirm purchase**.

Target application: [saucedemo.com](https://www.saucedemo.com)

## Approach

Instead of exhaustive coverage, effort is allocated by **risk (Likelihood × Impact)**:
- **High risk** areas (login, cart, checkout validation, totals, confirmation) → automated now, full assertion depth.
- **Medium/Low risk** areas (cancel flow, sorting, special test users, cross-browser) → deferred to manual/exploratory testing or later automation phases.

## What's in this repo

| File | Contents |
|---|---|
| `SauceDemo_Risk_Based_QA_Compact.md` | Risk map, readiness gate, flow pseudocode, prioritized test cases, cleanup steps, sign-off checklist |

## Contents at a glance

1. **Risk Map** — feature areas rated by likelihood/impact, with a clear automate-now vs. later call
2. **Readiness Gate** — the minimum questions that must be answered before automation starts
3. **Flow** — pseudocode for the end-to-end journey
4. **Test Cases** — 15 cases (TC-01–TC-15), each tagged with risk tier and automation timing
5. **Cleanup** — state/session hygiene between test runs
6. **Sign-off Checklist** — final review gate before automation build begins

## How to use

- Treat the risk map as a living document — re-rate areas as the app changes or defects surface.
- Automate High-risk test cases first; hold Medium/Low for later phases per the risk map's guidance.
- Update the readiness gate answers whenever the environment, accounts, or acceptance criteria change.
