# FAANG Release & Deployment

## Techniques

- Continuous deployment
- Feature gating
- Canary releases
- Testing in production
- Data-driven release processes

\---

- Intent-based (or declarative) approaches ("Desire Driven Deployment")
- "Manual" release process UI where pieces get automated as confidence grows
- Double writing

## Facebook (Meta)

### References

- \[Rapid Release at Massive Scale\](<https://engineering.fb.com/2017/08/31/web/rapid-release-at-massive-scale/>)
- \[Secret to Facebook's Hacker Engineering Culture\](<https://launchdarkly.com/blog/secret-to-facebooks-hacker-engineering-culture/>)

### Details

- Gatekeeper
  - Feature flag to get feedback and iterate quickly
  - Separate deployment from rollout unlocks
  - "Engineers can make a change, get feedback from thousands of employees using the change, and roll it back in an hour."
  - thousands of internal users (!!)
  - "if the engineer has planned for it in the code”
- Frontend pushed 3x/day using a simple "master and release branch strategy"
  - Cherry picks (500-700/day) performed from main branch into release branch and released
  - weekly release branch cut with any non-cherry-picked changes
  - technique scaled from handful of engineers to thousands
  - **required human release engineers** in addition to tools and automated systems
  - limit reached with \~1000 diffs/day and weekly push with \~10,000 diffs
  - Began gradual shift to quasi-continuous "push from master" system. Within a year, 100% of production web servers run code deployed directly from master

![GF0oPwHBDYx4GjgBAAAAAAAQG0oVbj0JAAAB.webp](.attachments.301744/GF0oPwHBDYx4GjgBAAAAAAAQG0oVbj0JAAAB.webp)

### Themes

- "Move Fast and Break Things" => "Move Fast With Stable Infra"
- Goal: enable developers to get code out quickly and correctly in safe, small, incremental steps
- "without rigidly adhering to any one \[technique\] in particular" (flexible, pragmatic)
- "shadow production" and "dark launches" (expose features to load without exposing them to users)
- Data-informed (release) decisions: "we expect this change to affect this metric; if it doesn't, it's gone" -- tie individual changes to specific metrics

## Amazon

### References

\- \[Going faster with continuous delivery\](<https://aws.amazon.com/builders-library/going-faster-with-continuous-delivery/>)

### Themes

- Deployment hygiene - automated tests to validate newly deployed resources & automatically rollback
- Testing: automate unit (style checks, coverage, code complexity, etc.), integration (failure injection, automated browser testing, etc.) & pre-production testing. Insist on **load and security testing** preferably in pipelines 
  - "pre-production testing": environment which uses the system's production configuration, including production data stores and backends. accessible to owning engineers, but never serve user traffic. this (a) ensures prod config allows connections to all resources and (b) ensures correct interacts with APIs of production services required
- Validating in production. Deploy to "cells" (completely independent instance of a service), gradual rollout. Synthetic traffic, tests which run at least every minute in production (verifies that running processes are health, dependencies are testing, etc)
- Controlling release of software (with metrics, time windows, and safety checks). Pipelines can be configured to prevent actions (deployments) when an alarm is fired based on metric changes. Allow overrides (for the fix, for example). Time windows during which changes are allowed through a pipeline. Halt pipeline based on contents of build artifact (specific git hashes, package dependency, etc)
- Visual interface to release process (i.e. pipeline dashboard). Started with every button being manual, but as confidence grew steps got automated
- Key takeaways:
  - automation give\[s\] engineers back time by removing frustrating, error prone, and laborious manual work
  - continuous deployment has a positive impact on quality
  - as complexity grows, the effort of managing release processes meets (and exceeds) effort of adding customer value \[...without proper automation\]
- We started by measuring where our pain was, addressing it, and iterating. To make this work sustainable, we needed to do it incrementally, and celebrate the improvements over time.

## Netflix

### References

\- \[Evolving how netflix builds, maintains, and operaters their Spinnaker distribution\](<https://blog.spinnaker.io/evolving-how-netflix-builds-maintains-and-operates-their-spinnaker-distribution-33c844d0102c>)

### Details

- Internal multi-cloud continuous delivery platform (Spinnaker)
- "understand application delivery requirements, not just a list of individual steps"
- for infra:
  - "I need XX cpu and YY memory" or "I need XX response time" vs "I need 3 nodes of XYZ instance type"
  - allows auto-remediation as instance types/load/etc. changes
- for delivery:
  - not "run xy merge, then xy deploy, then xy security scan, etc"
  - rather, environments are defined by what requirements must be met ("xy security scan has passed", "test xyz validation has been completed", etc.)
  - allows platform to determine the steps
  - this means: (1) owners can focus only on requirements that must be satisfied to give them confidence, (2) CD flow uses familiar concepts like artifacts, environments, constraints vs. jobs and other primitives, (3) guarantees supported/paved path for satisfying requirements vs bespoke tasks
- "Developers don't fail, the \[automation\] tools do", i.e. system shouldn't let "wrong" things happen

## Google

### References

- \[Software Engineering at Google (ch. 24)\](<https://abseil.io/resources/swe-book/html/ch24.html>)
- \[Branch Management - SWE Book, ch. 16\](<https://abseil.io/resources/swe-book/html/ch16.html#branch_management>)

### Details

- Velocity ("is a team sport")
  - If your releases are costly and sometimes risky, the *instinct* is to slow down your release cadence and increase your stability period
  - The *answer* is to reduce cost, increase discipline, and make the risks more incremental
  - resist the obvious operational fixes (reverting to a traditional planning model, adding more governance/oversight to the development process, implementing risk reviews, rewarding low-risk/low-value features)
  - invest in long-term architectural changes (microservices, rewrite to integrate desired modularity)
- Change Isolation
  - make sure engineers **"flag guard" *all changes***
  - phased rollouts as part of the feature flag system
- Striving for agility ("release train")
  - "No binary is perfect" (are "lines in an ad moving 2px" and "shading of a box" the same as our concerns?)
  - Have KPI metrics which provide clear thresholds for launch and allow for imperfection
  - "Meet your deadline" (if you are late for the release train, it will leave without you)
  - consistent schedule, missing a train means the next one can be caught "within hours"
- "We believe that a version control policy that makes extensive use of dev branches as a means toward product stability is inherently misguided."
  - The same set of commits are going to be merged to trunk eventually.
  - Small merges are easier than big ones.
  - Merges done by the engineer who authored those changes are easier than batching unrelated changes and merging later (which will happen eventually if a team is sharing a dev branch)
  - coordinating merge operations becomes significantly more expensive (and possibly riskier)
- Release Branches
  - generally benign
  - use cherry picking from trunk
  - do not merge back to trunk -- abandon at next release
  - that said, practically non-existent in highest functioning tech orgs (due to release velocity) 

### Themes

- "Always be deploying"
- Over time, smaller batches of changes result in higher quality (*faster is safer*)
  - Agility: Release frequent small batches
  - Automation: Reduce/remove repetitive overhead
  - Isolation: Strive for modular architecture (isolate changes)
  - Reliability: Measure key health indicators (crashes, latency) and improve them
  - Data-drive decision making: A/B testing on health metrics
  - Phased rollout: to a few users before shipping to everyone
- "One Version"
  - monorepo of *everything*
  - For every dependency in the repository, there is only one version which can be chosen. 
  - Third party == one version allowed
  - internal == no forking without renaming; old & new packages must be safely intermixable (i.e. if you have "app-foo-1" you can also use "app-foo-2" with no problems)
  - Any policy system that allows for multiple versions in the same codebase is allowing for the possibility \[of costly incompatibilities\]
  - "developers must not have a *choice* when adding a dependency onto some library that is already in use in the organization."
  - violations of the One-Version Rule lead to merge strategy discussions, diamond dependencies, lost work, and wasted effort.
- "(nearly) no long-lived branches"
  - "reduce work in progress" -- dev branches == pending work == work in progress
  - **"broad/shallow changes that are applied across the codebase are already a massive (often tedious) undertaking when modifying everything checked in to the trunk branch. Having an unbounded number of additional dev branches that might need to be refactored at the same time would be an awfully large tax on executing those types of changes"**
- "No Haunted Graveyards"
  - A haunted  graveyard in this  sense is a system that is so ancient, obtuse, or complex that no one dares enter it.
  - often business-critical systems that are frozen in time because any attempt to change them could cause the system to fail in incomprehensible ways, costing the business real money

### Bonus Content

- "large scale changes", i.e. auto-tasker at google ("rosie"): <https://abseil.io/resources/swe-book/html/ch22.html#large-scale_changes>
  - automatically skip "recently flaky" tests when validating LSCs
  - Instead of reviewing each change individually, global reviewers use a separate set of pattern-based tooling to review each of the changes and automatically approve ones that meet their expectations. Thus, they need to manually examine only a small subset that are anomalous because of merge conflicts or tooling malfunctions
