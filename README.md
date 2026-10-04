# Financial data API monitor

A demonstration of scheduled API checks that record what a customer can
retrieve and compare it with expected releases, records, and revisions.

I'm [Nathan Slaughter](https://nathanslaughter.com/), a software engineer whose
work includes observability, data pipelines, and financial systems. I help data
teams investigate delivery from the customer's point of view, producing
findings that can be inspected and reproduced.

**Status:** Project brief. This repository currently contains this README.
The monitor, configuration, example reports, and fault scenarios are planned;
nothing described here has been implemented or run yet. I will run and inspect
each scenario before describing its results. The data is synthetic, and this is
a demonstration project, not client work.

## What this project demonstrates

This is the supporting example for my API data delivery monitoring work: a
fixed-scope review of agreed customer requests over an observation period,
ending in a report a provider's team can investigate. It is meant to show:

- **Checks from the customer's side.** Requests use ordinary customer access
  and run independently of the API's internal telemetry.
- **Expectations with a source.** Each check compares the response with a
  release calendar, an expected set of identities, and values where known, and
  records where each expectation came from.
- **Evidence behind every finding.** Observations keep their timing, outcome,
  returned identities, and supporting responses, so a finding can be
  reproduced.
- **Conclusions limited to the evidence.** Confirmed discrepancies stay
  separate from suspicious changes, and observed customer impact stays separate
  from any inferred internal cause.
- **Visible gaps.** Monitor downtime, failed access, and incomplete pagination
  appear in the report as limits on what could be observed.

The monitor is the third of four stages in a demonstration for financial data
providers. It checks the
[financial-data-api](https://github.com/nslaughter/financial-data-api)
with the same customer requests a reader can run through the
[Python](https://github.com/nslaughter/financial-data-sdk-python),
[Go](https://github.com/nslaughter/financial-data-sdk-go), or
[TypeScript](https://github.com/nslaughter/financial-data-sdk-ts) SDK. The
monitor itself will be written in Go.

## An HTTP success does not establish that the expected data arrived

An API can return a successful response while still serving yesterday's
release. It can also return the expected number of rows while repeating one
observation and omitting another. Establishing the discrepancy requires an
expectation about the data and evidence of the response that was received.

The first demonstration will check the API using ordinary customer access. It
will work from a fictional release schedule and known fixture records: the
August value of an activity index is released as 102.4 on September 3, 2026,
and revised to 102.1 on September 10. Scheduled publication, confirmed source
publication, and first observation at the API remain distinct.

## How a delivery check will become an inspectable finding

1. Define a customer request, its expected records or release, and the source
   of that expectation. Agree the polling interval and request budget.
2. Run the request, including pagination, and retain its timing, outcome,
   returned identities, and supporting response evidence.
3. Compare the result with the expectation and record the discrepancy, affected
   workflow, and steps needed to reproduce it.
4. Repeat the request after the fixture is restored and show whether the
   expected result can be retrieved again.

## What a check records

Each check's configuration defines:

- the customer request: endpoint, filters, pagination, historical queries, and
  the datasets the monitoring credential can access;
- the release calendar and the source of each expectation;
- the expected identities, and values where they are known;
- the schedule, including closer observation around release windows, within
  an agreed request budget.

Each run records the check's start and finish, request identity, HTTP status,
latency, returned observation periods and revision identities, whether each
expected record was present, and a reference to the supporting response. All
times use UTC. Credentials are kept out of the results.

## Questions the report answers

- Was an expected release observed by the agreed deadline? Which was the last
  successful check without it, and the first that returned it?
- Do the returned identities match the expected set? Are records absent,
  repeated across pages, or inconsistent between related endpoints?
- Are revisions visible through the documented queries? Do time fields, units,
  and response shapes agree with the API contract?
- Which checks failed or could not run, and which observations need the
  provider's investigation?

## Fault scenarios and expected outcomes

| Scenario | What the monitor should record |
| --- | --- |
| Release delayed past its deadline | The last check without the release, the first check with it, and a finding against the deadline. |
| Expected observation missing | A finding naming the absent identity while the HTTP requests succeed. |
| Record repeated across pages | Repetition detected by identity, even though the row count matches. |
| Stale revision served | A finding that the documented revision was not returned. |
| Normal revision | An expected change, not a defect. |
| Late source publication | A source delay, distinguished from a provider delay only where the source release can be established independently. |
| Monitor interrupted | A gap in the observation history, not a finding of missing data. |
| Throttling, expired credentials, or incomplete pagination | A check that could not establish the result, kept separate from a confirmed absence. |
| Fixture restored | Subsequent checks retrieve the expected result again. |

The last case shows that the checks recognize the expected response again. It
is not a claim that monitoring repaired anything.

## The demonstration is complete when

- Checks detect a deliberately delayed release, a missing expected
  observation, a repeated paginated record, and a stale revision while the
  HTTP requests succeed.
- A normal revision and a late source are not misclassified.
- Interrupting the monitor itself produces a visible gap.
- Findings match independently prepared fixtures, and first observation and
  polling resolution are reported accurately.
- After the API responses are restored, subsequent checks succeed.

## What the repository will contain

- A local startup command and an example check configuration.
- A documented observation schema and an exportable results table.
- A sample findings report with supporting responses and reproduction steps.
- Reproducible fault scenarios with their expected outcomes.
- Documented coverage and unresolved cases, and a tagged version of the
  demonstrated monitor.

A dashboard is optional; the table and report should make the findings clear
without one.

## The report must show where its observations stop

A stopped monitor creates a gap in observation. Failed access or incomplete
pagination also limits what a check can establish. These outcomes will remain
visible in the report, alongside the endpoints, credentials, and period covered.
Polling can bound when a release was first observed; it cannot establish
uninterrupted availability between requests, or the exact moment every
customer could retrieve the data.

Coverage applies to the tested requests, credentials, location, and
observation period. Without a complete expected set, the report states the
narrower observation or unexplained change. Internal causes remain unresolved
unless separate evidence establishes them.

## Related projects and writing

- [financial-data-api](https://github.com/nslaughter/financial-data-api):
  the API these checks examine.
- [financial-data-sdk-python](https://github.com/nslaughter/financial-data-sdk-python),
  [financial-data-sdk-go](https://github.com/nslaughter/financial-data-sdk-go), and
  [financial-data-sdk-ts](https://github.com/nslaughter/financial-data-sdk-ts):
  clients a reader can use to run the same customer requests.
- *Checking whether your API delivers the expected data*: an article on this
  approach, in preparation. I'll link it here when it is published.
- *The timestamps that make financial data usable*: an article on the time
  fields these checks compare, also in preparation.

## Work with me on API data delivery monitoring

I offer scoped reviews of agreed customer requests over an observation period,
followed by a report your team can use to investigate missing, stale, or
unexpected data. Continuing checks and reporting can follow the initial review.
Deeper diagnosis or changes to ingestion and serving systems are scoped
separately.

[Discuss API data delivery monitoring](https://www.linkedin.com/in/nathan-slaughter)
with the API, expected releases, and customer delivery questions you want to
answer.
