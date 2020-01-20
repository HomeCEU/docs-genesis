# View Exam Results

When a learner completes a SCORM course with a passing grade the learner needs to access their exam results

## Actors
1. learner

## Flow
1. Learner is [enrolled into a SCORM course]
1. Learner [consumes SCORM content]
1. Learner completes exam with a passing grade
1. Learner navigates to the Transcript page
1. Learner selects the completed SCORM course
1. Learner clicks "view exam"
1. Learner sees their completed exam

## ToDo
- CEMS UI
  - Transcript
    - for scorm courses we need the "view exam" link to point to the Rustici registration which will re-launch the scorm content and they should be able to see their exam
