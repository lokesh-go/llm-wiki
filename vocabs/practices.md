# Engineering Practices Vocabulary

## TDD
Test-Driven Development — write failing test first, then write code to pass it, then refactor.
Cycle: Red → Green → Refactor.
Benefit: design emerges from tests; high coverage by default.
Related: [[BDD]], [[CI-CD]]

## BDD
Behaviour-Driven Development — write tests in business language (Given/When/Then).
Bridges communication between devs and non-technical stakeholders.
Tools: Cucumber, Jest (describe/it), Gherkin.
Related: [[TDD]]

## CI-CD
Continuous Integration / Continuous Delivery (or Deployment).
CI: merge and test code frequently. CD: automatically deploy passing builds.
Requires: automated tests, fast feedback loops, feature flags.
Related: [[trunk-based-development]], [[feature-flags]], [[blue-green-deployment]]

## trunk-based-development
All developers commit to a single main branch (trunk) frequently (at least daily).
Avoids long-lived feature branches. Requires [[feature-flags]] for incomplete work.
Related: [[CI-CD]], [[feature-flags]]

## feature-flags
Toggle features on/off at runtime without deploying new code.
Enables: trunk-based development, A/B testing, gradual rollouts, kill switches.
Related: [[trunk-based-development]], [[canary-release]]

## observability
Ability to understand system internal state from external outputs.
Three pillars: logs, metrics, traces.
Tools: Prometheus, Grafana, OpenTelemetry, Datadog.
Related: [[SLO-SLA-SLI]], [[chaos-engineering]]

## SLO-SLA-SLI
SLI: Service Level Indicator (what you measure, e.g., latency p99).
SLO: Service Level Objective (target, e.g., p99 < 200ms).
SLA: Service Level Agreement (contractual commitment with consequences).
Related: [[observability]], [[availability]]

## chaos-engineering
Intentionally injecting failures to discover weaknesses before they cause outages.
Netflix pioneered with Chaos Monkey.
Related: [[fault-tolerance]], [[observability]]

## blue-green-deployment
Two identical production environments (blue=live, green=new).
Switch traffic from blue to green after testing. Instant rollback possible.
Related: [[canary-release]], [[CI-CD]]

## canary-release
Gradually rolling out a new version to a small percentage of users first.
Monitor for errors before rolling out to everyone. Safer than [[blue-green-deployment]] for big changes.
Related: [[blue-green-deployment]], [[feature-flags]], [[CI-CD]]

## IaC
Infrastructure as Code — managing infrastructure through version-controlled config files.
Tools: Terraform, Pulumi, AWS CDK, Ansible.
Related: [[CI-CD]], [[Kubernetes]]
