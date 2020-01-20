# Record Failed Exam Attempt

When a learner failes the exam in a SCORM course we should record that failed attempt.

## Actors
1. learner

## Flow
1. Learner is [enrolled into a SCORM course]
1. Learner [consumes SCORM content]
1. Learner completes exam with a failing grade
1. Rustici sends xAPI messages to CEMS-Backend
1. CEMS-Backend interprets the xAPI messages and records
    - exam
      - questionId
      - questionText
      - answerId
      - score

# ToDo
- CEMS Backend
  - Endpoint to receive xAPI messages from Rustici
  - Library to interpret xAPI messages
    - record exam questions and answers

[enrolled into a SCORM course]: UseCase/ScormCourseEnrollment.md
[consumes SCORM content]: UseCase/ConsumeScormContent.md
