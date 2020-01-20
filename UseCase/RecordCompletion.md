# Record Completion UseCase

When a learner completes a course with a passing grade we need to record that in our system

## Actors
1. learner

## Flow
1. Learner is [enrolled into a SCORM course]
1. Learner [consumes SCORM content]
1. Learner completes exam with a passing grade
1. Rustici sends xAPI messages to CEMS-Backend
1. CEMS-Backend interprets the xAPI messages and records
    - exam
      - questionId
      - questionText
      - answerId
1. CEMS-Backend sends completion record to `certMan`

## ToDo
- CEMS Backend
  - Endpoint to receive xAPI messages from Rustici
  - Library to interpret xAPI messages
    - record exam questions and answers
  - Send completion records to `certMan`

[enrolled into a SCORM course]: UseCase/ScormCourseEnrollment.md
[consumes SCORM content]: UseCase/ConsumeScormContent.md
