# Exam Completion UseCase

When a learner completes an exam we need to record that in our system

## Actors
1. learner

## Flow
1. Learner is [enrolled into a SCORM course]
1. Learner [consumes SCORM content]
1. Learner completes exam
1. Rustici sends xAPI messages to CEMS-Backend
1. CEMS-Backend interprets the xAPI messages and records
    - exam
      - questionId
      - questionText
      - answerId
      - score

## Alternate Flows
### Learner Passes exam
6. CEMS-Backend sends completion record to `certMan`


[enrolled into a SCORM course]: UseCase/ScormCourseEnrollment.md
[consumes SCORM content]: UseCase/ConsumeScormContent.md
