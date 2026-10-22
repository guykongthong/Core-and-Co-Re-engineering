# Core&Co Re-engineering

A Software Metrics term project. We took a working webstore, Core&Co, measured it, then rebuilt it on a new stack and measured the rebuild with the same tools. The point was to choose the stack with numbers and check afterwards whether the choice paid off.

I was on a 2-person team and owned the backend and database. I also set up the SonarQube pipeline and directed and reviewed an AI-assisted Django REST Framework backend.

**Report:** [Software Metrics report (PDF)](https://portfoliowebsite-teal-ten.vercel.app/software-metrics-report.pdf)

## Two codebases

| Folder | What it is | Stack |
|---|---|---|
| `se262/` | The original Core&Co webstore, built earlier for a web programming and DevOps course. This is the baseline we measured. | Node.js, Express, EJS, MySQL, Docker Compose, GitHub Actions, AWS EC2 |
| `se327/` | The rebuild, done for the Software Metrics course. | Django REST Framework, PostgreSQL, React, Tailwind CSS |

The `se262/` README describes the original store in detail: the customer storefront, the admin back office, and the deployment pipeline.

## What we measured

**Stack selection.** Each candidate was scored with a weighted-criteria process covering COCOMO experience multipliers, test tooling, community size, and structure. Spring Boot scored 78, Django 75, and FastAPI 47. We chose Django anyway for its built-in features under the deadline, then compared those predictions with how the build actually went.

**Baseline and result.** The original was sized with function point analysis (213 unadjusted function points) and analysed with SonarQube. The rebuild was measured with the same tools:

| | Original | Rebuild |
|---|---|---|
| Test coverage | 0% | 98.7% |
| Bugs | 8 | 0 |
| Code smells per KLOC | 9.18 | 0.43 |
| Backend peak cyclomatic complexity | 66 | 19 |

The rebuilt backend has 578 tests.

## How the quality gate works

SonarQube Cloud runs in GitHub Actions on every pull request, with coverage imported from the backend tests. The quality gate blocks a merge if it would make those numbers worse, so an AI-assisted change gets the same automatic check as any other.

## Structure

```
se262/                 original webstore (baseline)
se327/
  backend/             Django REST Framework API
    apps/              accounts, analytics, campaigns, cart, catalog, orders
    common/            shared responses, errors, middleware
    config/            settings and routing
  frontend/            React and Tailwind CSS client
  docs/erd.md          database design
.github/workflows/     CI: frontend checks and SonarQube analysis
sonar-project.properties
```
