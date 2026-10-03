# Financial data API monitor

A demonstration of scheduled API checks that record what a customer can
retrieve and compare it with expected releases, records, and revisions.

I'm [Nathan Slaughter](https://nathanslaughter.com/), a software engineer whose
work includes observability, data pipelines, and financial systems. I help data
teams investigate delivery from the customer's point of view, producing
findings that can be inspected and reproduced.

**Status:** Project brief. This repository currently contains this README.
The monitor, configuration, example reports, and fault scenarios are planned.

## An HTTP success does not establish that the expected data arrived

An API can return a successful response while still serving yesterday's
release. It can also return the expected number of rows while repeating one
observation and omitting another. Establishing the discrepancy requires an
expectation about the data and evidence of the response that was received.

The first demonstration will check the
[financial-data-api](https://github.com/nslaughter/financial-data-api)
using ordinary customer access. It will work from a fictional release schedule
and known fixture records, keeping scheduled publication, confirmed source
publication, and first observation at the API distinct.

## How a delivery check will become an inspectable finding

1. Define a customer request, its expected records or release, and the source
   of that expectation. Agree the polling interval and request budget.
2. Run the request, including pagination, and retain its timing, outcome,
   returned identities, and supporting response evidence.
3. Compare the result with the expectation and record the discrepancy, affected
   workflow, and steps needed to reproduce it.
4. Repeat the request after the fixture is restored and show whether the
   expected result can be retrieved again.

The proposed cases cover delayed delivery, a missing observation, a repeated
row, and a stale revision. A normal revision and a late source will check that
the monitor can distinguish expected change from a delivery problem.

## The report must show where its observations stop

A stopped monitor creates a gap in observation. Failed access or incomplete
pagination also limits what a check can establish. These outcomes will remain
visible in the report, alongside the endpoints, credentials, and period covered.
Polling can bound when a release was first observed; it cannot establish
uninterrupted availability between requests.

The intended output is an exportable observation table and findings report,
with configuration, supporting responses, and repeatable fault scenarios.
Internal causes remain unresolved unless separate evidence establishes them.

## Work with me on API data delivery monitoring

I offer scoped reviews of agreed customer requests over an observation period,
followed by a report your team can use to investigate missing, stale, or
unexpected data. Continuing checks and reporting can follow the initial review.

[Discuss API data delivery monitoring](https://nathanslaughter.com/)
with the API, expected releases, and customer delivery questions you want to
answer.
