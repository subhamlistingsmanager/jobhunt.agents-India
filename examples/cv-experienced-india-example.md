# CV — Arjun Menon

<!--
  India EXPERIENCED CV example (fictional). ~5 years, backend/full-stack engineer,
  Bengaluru. Pairs with templates/cv-experienced-india.html.

  Section order matches the template: Professional Summary -> Core Skills ->
  Work Experience -> Key Achievements -> Education -> Certifications ->
  Additional Details. generate-pdf.mjs validates that the rendered HTML keeps
  the same section order as this markdown, so keep them in sync.

  Indian conventions shown: +91 phone, city + state, CGPA, ₹ LPA CTC and
  notice period in the optional footer. No photo / DOB / marital status.
-->

**Target Role:** Backend Engineer | Java, Spring Boot, AWS
**Location:** Bengaluru, Karnataka
**Phone:** +91 98450 31276
**Email:** arjun.menon.dev@example.com
**LinkedIn:** linkedin.com/in/arjunmenon-dev

## Professional Summary

Backend engineer with 5 years building payment and lending systems at Indian fintechs. Cut checkout API p99 latency from 1.8s to 240ms, scaled a UPI reconciliation service to 12M transactions/day, and migrated a monolith to 14 services with zero customer-facing downtime. Comfortable owning a service end to end — design, on-call, and cost.

## Core Skills

- **Languages:** Java, Kotlin, Python, SQL
- **Backend:** Spring Boot, Hibernate, REST, gRPC, Kafka
- **Data:** PostgreSQL, MySQL, Redis, Elasticsearch
- **Cloud & Infra:** AWS (EC2, RDS, SQS, Lambda), Docker, Kubernetes, Terraform
- **Practices:** System design, on-call/SRE, CI/CD, load testing

## Work Experience

### Razorpay — Bengaluru, Karnataka

**Senior Software Engineer (SDE-3), Payments**
Jul 2022 – Present

- Re-architected the checkout authorization service from a Rails monolith into 14 Spring Boot services; cut p99 latency from 1.8s to 240ms and deploy time from 40 min to 6 min
- Built a UPI reconciliation pipeline (Kafka + PostgreSQL) handling 12M transactions/day; reduced manual settlement breaks from ~900/day to under 30
- Owned on-call for 6 services; brought the payments error budget back within SLO (99.95%) after a quarter of breaches by adding idempotency keys and circuit breakers
- Cut AWS spend for the team by 22% (~₹38 lakh/year) by right-sizing RDS instances and moving batch jobs to spot fleets

### CRED — Bengaluru, Karnataka

**Software Engineer (SDE-2), Lending**
Jun 2020 – Jun 2022

- Built the loan-eligibility service integrating 3 bureaus (CIBIL, Experian, CRIF); served 4M+ users at 120ms median response
- Designed the EMI scheduler and NACH mandate flow; processed ₹600 crore in disbursals in the first year with a 0.2% failure rate
- Added contract tests and a staging replay harness that cut lending-related production incidents by 60% quarter over quarter

### Infosys — Pune, Maharashtra

**Systems Engineer**
Aug 2019 – May 2020

- Built REST APIs in Spring Boot for a US retail-banking client; wrote the integration test suite that lifted coverage from 41% to 78%
- Automated a manual nightly batch reconciliation, saving the ops team ~15 hours/week

## Key Achievements

- Filed a patent (pending) for a deduplication method in UPI reconciliation that cut duplicate-settlement risk at Razorpay
- Speaker at Bengaluru JUG 2023: "Killing tail latency in payment APIs" — 200+ attendees
- Won the Razorpay internal hackathon (2023) for a fraud-pattern detector later shipped to production
- Open-source: maintainer of a Spring Boot rate-limiter library, 1.2k GitHub stars

## Education

- **B.Tech, Computer Science & Engineering** — National Institute of Technology (NIT) Calicut, 2019. CGPA: 8.7/10
- **Class 12 (CBSE)** — Bhavans Vidya Mandir, Kochi, 2015. 93.4%
- **Class 10 (CBSE)** — Bhavans Vidya Mandir, Kochi, 2013. 95.2%

## Certifications

- AWS Certified Solutions Architect – Associate — Amazon Web Services, 2023
- Oracle Certified Professional, Java SE 11 — Oracle, 2021

## Additional Details

**Notice Period:** 60 days  |  **Current CTC:** ₹34 LPA  |  **Expected CTC:** ₹46 LPA  |  **Work Authorization:** Indian citizen
