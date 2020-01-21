# Consume SCORM Content UseCase

When enrolled into a course with SCORM content a learner will need to consume it.  They will consume the content though rustici.

## Actors
1. learner

## Flow
- Login
- Open Enrollments
- Select an enrollment with SCORM content
- Select the link for the video
- Rustici will progress learner though content
  - video
  - exam
  - survey (via survey monkey?)
- Rustici will send xAPI messages to CEMS Backend
