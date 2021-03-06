# CI Signal Lead Playbook

## Overview of CI Signal responsibilities

CI Signal lead assumes the responsibility of the quality gate for the release. This person is responsible for:
- Continuously monitoring various e2e tests in sig-release dashboards ([master-blocking](https://k8s-testgrid.appspot.com/sig-release-master-blocking), [master-informing](https://k8s-testgrid.appspot.com/sig-release-master-informing), `x.y-blocking` (x.y being the current release)) throughout the release cycle
- Providing early and ongoing signals on release and test health to both Release team and various SIGs
- Ensuring that all release blocking tests provide a clear Go/No-Go signal for the release
- Flagging regressions as close to source as possible i.e., as soon as the offending code was merged
- Filing issues proactively for test failures and flakes, triaging issues to appropriate SIG/owner, following up on status and verifying fixes on newer test runs
  - See also: [Working with SIGs outside sig-release](#working-with-sigs-outside-sig-release)
- Studying patterns/history of test health from previous releases and working closely with SIGs and test owners to
  - Understand and fix the test for the current release
  - Understand if the test needs to be release blocking
  - Work with SIG-Testing on any possible test infra improvement to help improve test pass rate
- Making recommendations to SIG-Release for promoting and demoting release blocking and merge blocking tests as per the [Blocking tests Proposal](https://docs.google.com/document/d/1kCDdmlpTnHPQt5z8JzODdFCc3T2D4MKR53twsDZu20c/edit)

The core responsibility of the CI Signal lead is to foster a culture of continuous test integration, fail-fast and fix-fast mindset and strive for continuously passing tests that provide true positive signal to the release team. To ensure that releases go out on-time with high quality it is absolutely critical to maintain a sustained focus on test health through the entire release cycle, as opposed to accepting destabilizing test failures during active enhancement development followed by a long test stabilization phase.

### Explicit detail is important:

- If you're looking for answer that's not in this document, please file an issue so we can keep the document current.
- Generally CI Signal lead errs on the side of filing an issue for each test failure or flake before following up with SIGs/owners. This way we don't _lose track of an issue_
- If a dashboard isn't listed here, or a test isn't on one of the listed dashboards, **_CI Signal lead is not looking at it_**

## Requirements

**Before continuing on to the CI Signal specific requirements listed below, please review and work through the tasks in the [Release Team Onboarding Guide](/release-team/release-team-onboarding.md).**

### Time Requirements

CI Signal is one of the more time-intensive roles on the release team, and as such is not recommended for candidates who do not have employer support for their work on the Release Team.  General time requirements for Leads and Shadows are:

- 1/2 hour to 2 hours a day, every workday, checking failing tests and following up on failing test and flake issues.
- Availability to attend the majority of Release Team (weekly) and Burndown meetings (daily during Code Freeze), subject to time zone appropriateness.
- Ability to follow-up on test fixes during Code Freeze at arbitrary times to ensure rapid turnaround.
- The time commitment becomes greater through the release cycle, peaking during Code Freeze.  In the last week of Code Freeze, Shadows should expect to spend at least 10 hours and leads at least 20 hours.

### Additional Requirements for Shadows

The following requirements are all things that Leads should already have, and Shadows should acquire, with coaching, within the first weeks of the release cycle:

- Have signed the contributor CLA for Kubernetes.
- Become a Kubernetes org member.  In many cases, this will be done with the sponsorship of the CI Signal Lead or Release Lead in the first week of the cycle.
  - The process to become one of these is in [our community membership ladder](https://github.com/kubernetes/community/blob/master/community-membership.md#requirements-for-outside-collaborators)
- General familiarity with our [test boards](https://testgrid.k8s.io/) and examining test results from automated tests.
- Willingness and ability to follow-up with contributors about test failures, on Slack, email, Discuss, and SIG meetings, as appropriate.
- Ability to file well-crafted Kubernetes issues, including labelling.
- General knowledge of our SIG system and SIGs' areas of responsibility.

Additionally, the following qualifications make a candidate more suitable for the CI Signal team, but are not requirements:

- Prior involvement with SIG Testing and the Test Infrastructure team.
- Experience with automated testing, CI/CD, quality engineering, and/or quality assurance.

### Additional Requirements for Leads

In addition to the above requirements for Shadows, most of which become prerequisites, CI Signal Leads must:

- Have the ability to add a milestone to issues, so must be a member of the [milestone maintainers](https://github.com/orgs/kubernetes/teams/kubernetes-milestone-maintainers)
- Have a working knowledge of our various test infrastructure tools, such as Testgrid, Triage, gubernator, Prow, and Tide.
- Signal lead need to understand what tests matter and generally how our testing infra is wired together.
  - He/she can ask previous CI Signal leads for advice
  - He/she can ask SIG-Testing for guidance

## Overview of tasks across release timeline

For any release, its schedule and activities/deliverable for each week will be published in the release directory, e.g: [1.11 schedule](https://github.com/kubernetes/sig-release/blob/master/releases/release-1.11/release-1.11.md#timeline). This section talks about specific CI Signal lead deliverable for each milestone in the release cycle.

### Pre Enhancement Freeze

Here are some good early deliverables from the CI Signal lead between start of the release to enhancement freeze.
- Maintain a master tracking sheet and keep it up-to-date with issues tracking any test failure/flake - [Sample sheet](https://docs.google.com/spreadsheets/d/1j2K8cxraSp8jZR2S-kJUT6GNjtXYU9hocNRiVUGZWvc/edit#gid=127492362)
- Copy over any open test issues from previous release (ask previous CI Signal lead for the tracker sheet) and follow up on them with owners
- Monitor [master-blocking](https://k8s-testgrid.appspot.com/sig-release-master-blocking) and [master-informing](https://testgrid.k8s.io/sig-release-master-informing) dashboards **twice a week** and ensure all failures and flakes are tracked via open issues
  - Make sure all open issues have a `priority/` label (see: [Priority Labels](#priority-labels)) and one of either the `kind/flake` or `kind/failing-test` label
  - Make sure the issue is assigned against the current milestone 1.x, using /milestone
  - Assign the issue to appropriate SIG using /sig label
  - If you are aware of the individual associated with the enhancement area or issue, @mention of individual(s) and SIG leads tends to result in faster turn around
  - Add @kubernetes/sig-foo-test-failures to draw SIG-foo’s attention to the issue
  - CC the release manager and bug triage lead
  - Post the test failure in SIG’s Slack channel to get help in routing the issue to the rightful owner(s)
  - [Sample test failure issue](https://github.com/kubernetes/kubernetes/issues/63611)
- Build and maintain a document of area experts / owners across SIGs for future needs e.g.: Scalability experts, upgrade test experts etc

#### **_Best Practice:_**
The SLA and involvement of signal lead at this stage might vary from release to release (and the CI Signal lead). However in an effort to establish an early baseline of the test health the signal lead can take an initial stab at the tests runs at the start of the release, open issues, gather and report on the current status. Post that, it might suffice to check on the tests **twice a week** due to high code churn and expected test instability.

### Enhancement Freeze to Burndown

Day to day tasks remain pretty much the same as before, with the following slight changes
- Monitor [master-blocking](https://k8s-testgrid.appspot.com/sig-release-master-blocking) dashboard **daily**
- Monitor [master-informing](https://k8s-testgrid.appspot.com/sig-release-master-informing) **every other day**
- Starting now, curate a **weekly CI Signal report** escalating current test failures and flakes and send to kubernetes-dev@googlegroups.com community. [See below](#reporting-status) for more details on the report format and contents

Increased attention on maintaining signal early in the release cycle goes a long way in reducing the risk of late-found issues destabilizing the release during code freeze.

### Burndown to Code Freeze

This is when things really begin to ramp up in the release cycle with active bug triaging and followup on possible release blocking issues to assess release health. Day to day tasks of CI Signal lead include
- Auditing test status of master-blocking, master-upgrade and release-1.x.y-blocking dashboards on **daily basis**
- Keeping issues' status upto-date on GitHub
- Working closely with SIGs, test owners, bug triage lead and Release team in triaging, punting and closing issues
- Updating issue tracker frequently so anyone on the release team can get to speed and help followup on open issues
- Continuing to send [weekly CI signal report](#reporting-status)

### During Code Freeze

- Continue best practices from Burndown stage. Monitor master-blocking, master-upgrade and release-1.x.y-blocking dashboards on **daily basis**
- Quickly escalate any new failures to release team and SIGs
- Continuing to send [weekly CI signal report](#reporting-status)

### Exit Code Freeze

- Once 1.x.y-rc.1 release branch is cut and master re-opens for next release PRs
  - continue **monitoring master-upgrade and 1.x.y-blocking dashboards on daily basis**
  - you need not monitor master-blocking on a daily basis. It is, however, worth keeping an eye on master-blocking especially before risky cherry-picks make their way into the release branch
- If tests have stabilized (they should have by now), you may stop sending the weekly CI report

## Special high risk test categories to monitor

Historically there are a couple of families of test that are hard to stabilize, regression prone and pose a high risk of delaying and/or derailing a release. As CI Signal lead it is highly recommended to pay close attention and extra caution when dealing with test failures in the following areas.

### Scalability tests
Scalability testing is inherently challenging and regressions in this area are potentially a huge project risk
- Requires lots and lots of servers running tests, and hence expensive
- Tests are long running, so especially hard/expensive/slow to resolve issues via Git bisection
- Examination of results is actually the bigger more expensive part of the situation

The following scalability jobs/tests could regress quite easily (due to seemingly unrelated PRs anywhere in k8s codebase), require constant manual monitoring/triaging and domain expertise to investigate and resolve.
- [master-blocking gce-cos-master-scalability-100](https://testgrid.k8s.io/sig-release-master-blocking#gce-cos-master-scalability-100)
- [master-informing gce-scale-correctness](https://testgrid.k8s.io/sig-release-master-informing#gce-master-scale-correctness)
- [master-informing gce-scale-performance](https://testgrid.k8s.io/sig-release-master-informing#gce-master-scale-performance)
- `release 1.xx-blocking gce-cos-1.xx-scalability-100`

The CI Signal lead should
- Continuously monitor these tests early in the release cycle, ensure issues are filed and escalated to the Release team and right owners in SIG-Scalability (@bobwise, @shyamjvs, @wojtek-t)
- Work with SIG-Scalability to understand if the failure is a product regression versus a test issue (flake) and in either case followup closely on a fix
- Additionally, it might help to monitor SIG-Scalability’s performance dashboard to flag if and when there is considerable performance degradation

Starting in 1.11, scalability tests are now blocking OSS presubmits. Specifically we are running performance tests on gce-100 and kubemark-500 setups. This is a step towards catching regressions sooner and stabilizing the release faster.

## Working with SIGs outside sig-release
2 scenarios that you will be involved in:

1. Identifying tests from sig-<name> that should be/are part of sig-release's blocking and informing dashboards. Those tests could be submitted as part of a new enhancement that sig-<name> is developing, or could be existing tests in blocking/informing dashboards.
Questions to ask sig-<name>:
  - Which e2e test jobs are release blocking for your SIG?
  - What is the process for making sure the SIG's test grid remains healthy and resolving test failures?
  - Would moving the e2e tests for the SIG into their own test jobs make maintaining those tests easier? If yes, consider placing them in a dashboard owned by sig-<name>.
  - Is there a playbook for how to resolve test failures and how to identify whether or not another SIG owns the resolution of the issue? If not, could you (sig-<name>) develop one?
  - What is the escalation point (email + slack) that will be responsible for keeping this test healthy?

1. Escalating test failures/flakes to sig-<name>. Expectations:
  - Each test must have an escalation point (email + slack).  The escalation point is responsible for keeping the test healthy.
  - Fixes for test failures caused by areas of ownership outside the responsibility of the escalation point should be coordinated with other teams by the test escalation point.
  - Escalation points are expected to be responsive within 24 hours, and to prioritize test failure issues over other issues.
    - If you don't see this happening, get in touch with them on slack or other means to ask for their support.

## Tips and Tricks of the game

### Checking test dashboards

- Quirk: if a job is listed as FAILING but doesn't have "Overall" as one of its ongoing failures, it's not actually failing. It might be "red" from some previous test runs failures and will clear up after a few "green" runs
- if a job is failing in one of the meta-stages (Up, Down, DumpClusterLogs, etc), find the owning SIG since it is a infra failure
- if a job is failing because a specific test case is failing, and that test case has a [sig-foo] in its name, tag SIG-foo in the issue and find appropriate owner within the SIG
- with unit test case failures, try to infer the SIG from the path or OWNERS files in it. Otherwise find the owning SIG to help
- with verify failures, try to infer the failure from the log. Otherwise find the owning SIG to help
- if a test case is failing in one job consistently, but not others, both the job owner and test case owner are responsible for identifying why this combination is different
- You can look at past history of the job/test (even as far back as multiple releases) by querying the [triage dashboard for specific job and/or test name](https://storage.googleapis.com/k8s-gubernator/triage/index.html)

### Priority Labels
Issues you create for test failures and flakes must be assigned a `priority` label, that is compatible with the priorities described in the [Issue Triage contributor guide](https://github.com/kubernetes/community/blob/master/contributors/guide/issue-triage.md#define-priority).

In the CI signal context, we're using priority labels to mean:

| priority | Description | Expectation |
| -- | -- | -- |
| `priority/critical-urgent` | Actively impacting release-blocking signal. Includes: consistently failing tests,  frequently (more than 20% of the time) flaking tests  in release-blocking dashboards. | Work with sigs for these to be worked on as soon as possible, prioritized over other work. |
| `priority/important-soon` | Negatively impacting release-blocking signal. Includes: Flakes (especially occurring >2% of the time) in release-blocking jobs, failures and flakes in release-informing jobs. | Work with sigs to resolve them soon, ideally before the end of this release cycle. |
| `priority/important-longterm` | Painful, results in manual workarounds/toil, limits developer productivity, but is of lower urgency and importance than issues in `priority/critical-urgent` or `priority/important-soon`. | In reality, there's a high chance these won't be finished, or even started within the current release. Work with sigs to ensure they are on their radar, and help find ways they can be broken down into smaller chunks. |

CI signal is not currently using `priority/backlog` or `priority/awaiting-more-evidence`.

### Routing test issues to SIG/owner

This can be done in 2 ways (i) including @kubernetes/sig-foo-test-failures teams in the issue files and (ii) going to the #sig-foo slack channel if you need help routing the failure to appropriate owner.

If a job as a whole is failing, file a v1.y issue in kubernetes/kubernetes titled:`[job failure] job name issue`
```
/priority critical-urgent
/kind failing-test
@kubernetes/sig-FOO-test-failures

This job is on the [sig-release-master-blocking dashboard](https://k8s-testgrid.appspot.com/sig-release-master-blocking),
and prevents us from cutting [WHICH RELEASE] (kubernetes/sig-release#NNN). Is there work ongoing to bring this job back to green?

https://k8s-testgrid.appspot.com/sig-sig-release-master-blocking#[THE FAILING JOB]

...any additional debugging info I can offer...
```
Examples:
- [[job failure] ci-kubernetes-e2e-kubeadm-gce](https://github.com/kubernetes/kubernetes/issues/54905)

If a test case is failing, file a v1.y milestone issue in kubernetes/kubernetes titled: `[e2e failure] full test case name`
```
/priority critical-urgent
/kind failing-test
@kubernetes/sig-FOO-test-failures

This test case has been failing [SINCE WHEN OR FOR HOW LONG] and affects [WHICH JOBS]: [triage report](LINK TO TRIAGE REPORT)

This is affecting [WHICH JOBS] on the [sig-release-master-blocking dashboard](https://k8s-testgrid.appspot.com/sig-release-master-blocking),
and prevents us from cutting [WHICH RELEASE] (kubernetes/sig-release#NNN). Is there work ongoing to bring this test back to green?

...any additional debugging info I can offer...

eg: if there's a really obvious single cluster / cause of failures
[triage cluster 12345](LINK TO TRIAGE CLUSTER)
---
the text from the triage cluster
---
```
Examples:
- [[e2e failure] [sig-network] Networking Granular Checks: Services [Slow] should function for client IP based session affinity: udp](https://github.com/kubernetes/kubernetes/issues/54524)
- [[e2e failure] [sig-network] Networking Granular Checks: Services [Slow] should function for client IP based session affinity: http](https://github.com/kubernetes/kubernetes/issues/54571)

### Monitoring Commits for test failure triangulation
Yet another effective way for a signal lead to stay on top of code churn and regression is by monitoring commits to master and specific branch periodically.
- Once a day look at https://github.com/kubernetes/kubernetes/commits/master/ for master or a specific branch
- Look for any new end-to-end tests added by a PR

This helps us
- Stay aware of tests’ history and identify ownership when they start failing
- Identify possible culprit PRs incase of a regression
- Usually if the volume of things merged is very low, then that’s a signal that something is terribly broken as well

## Reporting Status
Since 1.11 we started curating and broadcasting a weekly CI Signal report to the entire kubernetes developer community highlighting
  - Top failing and flaky jobs and tests
  - New failing and flaky jobs and tests in the past week
  - Call out SIGs actively fixing and ignoring issues
  - Call out potential release blocking issues
  - Call out upcoming key dates e.g., Code freeze, thaw etc
  - [Link to the previous reports](https://docs.google.com/document/d/1Xesru5QkBxxl8WOirNcWI4ho3exqfQBGz6mYJeXOi3U/edit#heading=h.qs3772g2kexl)
  - CCing individual SIGs (kubernetes-sig-foo@googlegroups.com) that own failing tests help in getting their attention

[Sample Weekly CI Signal Report](https://groups.google.com/d/msg/kubernetes-dev/nO8aKz3tavE/-aBpStu4CQAJ)

Early and continuous reporting of test health proved greatly effective in rallying SIG attention to test failures and stabilizing the release much early in the release cycle.
