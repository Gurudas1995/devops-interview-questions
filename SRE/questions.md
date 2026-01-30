# Interview Questions and Answers on SRE

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [List All git Commands.](#List-All-git-Commands) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |



1. ### What is Site Reliability Engineering?
Site Reliability Engineering (SRE) is applying software engineering principles to operations to build scalable, reliable systems. SRE focuses on defining reliability through SLIs and SLOs, automating operations, reducing toil and managingrisk via error budgets rather than reacting to incidents manually.

2. ### Expalin difference between SLI, SLO and SLA?
 - *SLI* : Quantative measure of level of services such as latency, availability, error rate etc..(e.g. ratio of good events to total events)
 - *SLO* : Target Value or range of value for a service measured by SLI (e.g. 99.9% availability)
 - *SLA* : It's an contractual commitement with penalties between customer and service proivder.

3. ### What is an error budget?
Error budget is the acceptable amount of unreliability. If a service has a 99/9% SLO, the error budget is 0.1%. When the budget is exhausted, feature releases stop and focus shifts to reliability.
