# DevOps analysis of The Company AB

## 1. The flow
- Team of three people developing a native iOS app
- A developer takes a Jira ticket and spends around 3 days implementing it.
- The pull request waits around 2 days for approval from two reviewers. The actual review takes around 30 minutes.
- After merging to main, the team manually tests new features on test devices during a scheduled 1-hour test session every Tuesday.
- If testing finds a bug, a new Jira ticket is created. It must be fixed and reviewed before the release. If it cannot be fixed in time, the release is postponed. Bugs are found in approximately 1 out of 3 test sessions.
- On Wednesday, the app is manually archived from a developer's Mac, taking around 15 minutes, and uploaded to App Store Connect.
- Apple typically takes around 24 hours to review the release.
- Once approved, a developer manually releases the app to users on Thursday (the next day).
- The team releases the app approximately once a week.
- There is no actual rollback mechanism. If a serious problem reaches production, a new fix must be developed, reviewed, archived and submitted to Apple again. Apple review typically takes around 24 hours, after which the new version is manually released. Users on the broken version are prevented from logging in until they update. The total recovery time is not measured, but it is expected to take at least one day plus the time needed to develop and review the fix.

## 2. Value stream map
| Step | Processing time | Waiting time | Right first time |
|---|---:|---:|---:|
| Development | ~3 days | — | — |
| PR approval queue | — | ~2 days | — |
| Code review | ~30 min | — | — |
| Waiting for test session | — | Up to ~1 week | — |
| Manual testing | ~1 hour | — | ~67% |
| Fix bugs (if found) | ~1 day | — | — |
| Archive & upload | ~15 min | — | — |
| Apple review | — | ~24 hours | — |
| Manual release | ~15 min | — | — |

**Total lead time**
Total processing time: ~4 days and 2 hours = 98 hours
Total waiting time: ~10 days = 240 hours

Total lead time = 98 + 240 = 338 hours

**Share of value creating time**
Share of value creating time = 98 / 338 = 0.29 = 29%

## 3. The three longest waits

### 1. Waiting for the Tuesday test session: up to ~1 week
**Why it exists:** Testing is performed manually on test devices during a scheduled one hour session every Tuesday. There are not enough automated tests, so the team relies on this weekly session to test new features.
sts, so the team relies on this weekly session to test new features.

**What it costs:** A completed change may sit in `main` for several days before it can be tested. This increases lead time and can delay the release if bugs are found.

### 2. Pull request approval: ~2 days
**Why it exists:** Two reviewers need to approve the pull request before it can be merged.

**What it costs:** The code is finished but cannot be merged. The actual review only takes ~30 minutes, so most of the ~2 days is waiting time.

### 3. Apple review: ~24 hours
**Why it exists:** Every release needs to be reviewed by Apple before it can be released to users.

**What it costs:** The team cannot release the app until Apple approves it. This waiting time is outside the team's direct control and can take up to 48 hours.

## 4. DORA metrics for this flow
**Deployment frequency**
One deployment per week

**Lead time for changes**
Approximately 14 days (338 hours)

**Change failure rate**
Cannot be determined from the available data. Bugs are found in approximately one out of three (33%) test sessions but production failure data is needed to calculate the actual metric.

**Time to restore service**
Apple review takes approximately 24 hours, followed by manual release. Recovery is expected to take at least one day plus development and review time, but the actual restoration time is unknown.

## 5. Assessment against the quality criteria
## 5. Assessment against the quality criteria

### Development

| Criterion | Assessment | Evidence from the mapping |
|---|---|---|
| Correctness | Partially met | Features are tested manually on devices for approximately one hour every Tuesday. However, there are not enough automated tests and bugs are found in approximately one out of three test sessions. |
| Maintainability | Cannot be determined | The mapping contains no information about code structure, documentation or maintainability. |
| Traceability | Met | Changes is tracked through Jira tickets and GitHub. Pull requests require approval from two reviewers before it is merged into main. |
| Secure by default | Partially met | Apple requires certificates and appropriate signing credentials to distribute the app, restricting who can release it. However, this analysis does not cover other security measures, such as dependency management or secret handling. |

### Operations

| Criterion | Assessment | Evidence from the mapping |
|---|---|---|
| Availability | Partially met | The app is released approximately once a week. However, there is no rollback mechanism, and users on a broken version are prevented from logging in until they update. Recovering from a serious production issue requires a new release, including Apple's review. |
| Observability | Met | The team uses logging, metrics and alerts to monitor the application and detect issues. These tools help the team investigate problems and understand what is happening in production. |
| Recoverability | Not met | There is no actual rollback mechanism. A serious production issue requires a fix to be developed, reviewed, archived, submitted to Apple and approved before it can be released. Recovery takes at least approximately one day plus development and review time. |
| Predictability | Partially met | The team follows a weekly release schedule, with testing on Tuesdays and releases on Thursdays. However, testing and release steps are manual, there are no automated tests, and Apple review can take up to 48 hours. |

## 6. What I would change first, and why
I would focus on automating testing first, since the team currently relies on a one hour manual test session every Tuesday. This takes up developers time and can be less consistent than automated testing, as manual testing depends on human execution and can miss bugs. It also creates a waiting time of up to one week before changes can be tested.

Automated tests would provide faster feedback after changes are merged, reduce dependence on the weekly test session and potentially shorten lead time. They could also help the team detect bugs earlier and reduce the risk of delaying weekly releases.