# How Amazon Q Developer Could Support the ADM

> **Alpha.** This tooling guidance reflects the current state of the AWS ecosystem and will be updated as tooling matures and practical experience grows.

This note sets out how Amazon Q Developer could support the Agentic Delivery Model in an AWS-heavy enterprise software development context.

It is a practical implementation note, not a product endorsement. It focuses on enterprise software development rather than general cloud operations.

## 1. Scope and caveat

Amazon Q Developer is a credible ADM component in enterprises where AWS is already the dominant engineering environment.

AWS positions Amazon Q Developer as supporting work across the software development lifecycle, including coding, code reviews, testing, debugging and modernisation. AWS has also published capabilities where the agent runs build and test scripts to validate generated code before human review.

This makes Amazon Q Developer a strong implementation-layer option, especially in AWS-heavy estates.

## 2. Where Amazon Q Developer fits in the ADM

Amazon Q Developer is strongest in:

1. implementation and code change assistance
2. build and test validation before review
3. code review support
4. AWS-aware development and modernisation work

For the ADM, that means it fits best as part of the execution layer rather than as the complete operating model.

## 3. Enterprise strengths for the ADM

For the ADM, Amazon Q Developer is particularly useful where:

- teams build heavily on AWS services
- AWS context matters during implementation
- IDE-centred development is still the norm
- application modernisation and transformation work are important
- build and test validation before review has clear value

AWS has also made Amazon Q Developer available across common enterprise development surfaces such as VS Code and JetBrains.

## 4. Where Amazon Q Developer needs complements

Amazon Q Developer is not, by itself, the full enterprise delivery stack the ADM needs.

Most organisations would still need complementary tooling for:

- backlog and business workflow
- pull request governance outside the IDE where required
- broader enterprise policy and audit management
- multi-vendor model governance

In practice, Amazon Q Developer is likely to be one part of a broader AWS-heavy enterprise toolchain, not the only platform involved.

## 5. Reference operating pattern

A practical AWS-heavy ADM pattern could look like this:

1. Intent Specifications are approved through the team's normal backlog and governance flow.
2. Amazon Q Developer supports implementation in the IDE or CLI.
3. The agent runs selected build and test commands before human review.
4. Human reviewers assess the resulting changes through the team's repository and governance process.
5. AWS service context and modernisation capability improve implementation quality in AWS-heavy systems.

This is especially relevant where the main delivery challenge is building and evolving software tightly coupled to AWS services.

## 6. Recommended position in the ADM repository

The ADM should present Amazon Q Developer as a strong enterprise scenario for AWS-heavy engineering estates.

It is best described as a strong implementation-layer and modernisation-layer option that will usually sit alongside other enterprise delivery controls rather than replace them.

## 7. Source notes

This note is based on public AWS documentation and announcements, including:

- [Amazon Q Developer is now generally available](https://aws.amazon.com/about-aws/whats-new/2024/04/amazon-q-developer-generally-available/)
- [Amazon Q Developer agent now runs builds and tests to validate generated code in real time](https://aws.amazon.com/about-aws/whats-new/2025/01/amazon-q-developer-agent-builds-tests-validate-generated-code-real-time/)
- [Amazon Q Developer announces a new agentic coding experience in the IDE](https://aws.amazon.com/about-aws/whats-new/2025/05/amazon-q-developer-agentic-coding-experience-ide/)
- [Amazon Q Developer can now automate code reviews](https://aws.amazon.com/about-aws/whats-new/2024/12/amazon-q-developer-automate-code-reviews/)
- [Amazon Q Developer build overview](https://aws.amazon.com/q/developer/build/)

AWS product scope and terminology are evolving quickly. This note should be updated as the platform matures.