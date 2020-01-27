## Phase 1 Use Cases
- [Course Lookup]
- [Copy Course]
- [Add SCO to course]
- [Generate proxy files for company]
- [Get course detail and proxy sco files]
- [Consume SCORM content]
- [Exam completions]
- [Download Certificate of Completion (COC)]

## Phase 2 Use Cases
- [SCORM course enrollment]

## Task List
- [TaskList]

## NOT required for genesis
- CEMS users do not need sco file support
  - no UI updates required
  - create script to roll out new sco courses per company (replace in company catalog)


## Business Rules
- SCO files can not be updated, a new course will have to be created if there is a change to the sco file (enforced by Rustici)
- SCO file can not be added to a course which already has enrollments, must create a new course.
- Course SCO files can not be deleted unless there has never been a course registration/enrollment

## Notes
- SCORM course enrollment
  - we will need to modify an endpoint to provide UI with link to rustici and some kind of flag so it knows to hide take exam, maybe isScorm or something? or maybe pass a null examId
  - make examId not required in database?
- external registration
  - we don't yet know how the external LMS will request a certificate, we assume when the learner starts the sco rustici generates a registration id.  if it passes that id back to the external LMS then they can use that to request the certificate, else they will have to request it by companyid, learnerid, courseid, date

## Questions
- Rustici
  - once we create a dispatch is the link to the proxy sco file public so we can give it out directly or do we have to download the proxy sco files and serve them from cems?
- CEMS UI
  - Enrollments
    - How will UI know to send learner to /play/courseId vs Rustici ?
    - How will UI know to not display 'Take Exam'
  - Transcript
    - How will UI know when to send the user to kohana vs Rustici?

[Course Lookup]: UseCase/CourseLookup.md
[Add SCO to course]: UseCase/AddScoToCourse.md
[Generate proxy files for company]: UseCase/GenerateCompanyProxyScoFiles.md
[Get course detail and proxy sco files]: UseCase/GetCourseDetailAndProxyScoFiles.md
[SCORM course enrollment]: UseCase/ScormCourseEnrollment.md
[Consume SCORM content]: UseCase/ConsumeScormContent.md
[Exam completions]: UseCase/ExamCompletion.md
[View Exam Results]: UseCase/ViewExamResults.md
[Download Certificate of Completion (COC)]: UseCase/DownloadCOC.md
[TaskList]: ToDo.md
