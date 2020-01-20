## Use Cases
- [Add SCO to course]
- [Generate proxy files for company]
- [External LMS needs course detail and proxy sco files]
- [SCORM course enrollment]
- [Consume SCORM content]
- [Record completions]
- [Download Certificate of Completion (COC)]

## Notes
- Generate proxy files for company
  - if the proxy sco file link from rusitci is open we can store their link rather than downloading the proxy sco file our self, in which case the additional table and separate process won't be necessary.
- SCORM course enrollment
  - hide take exam on enrollments page for SCORM courses
  - we will need to modify an endpoint to provide UI with link to rustici and some kind of flag so it knows to hide take exam, maybe isScorm or something? or maybe pass a null examId
  - make examId not required in database?


[Add SCO to course]: UseCase/AddScoToCourse.md
[Generate proxy files for company]: UseCase/GenerateCompanyProxyScoFiles.md
[External LMS needs course detail and proxy sco files]: UseCase/CourseDetailAndProxyScoFiles.md
[SCORM course enrollment]: UseCase/ScormCourseEnrollment.md
[Consume SCORM content]: UseCase/ConsumeScormContent.md
[Record completions]: UseCase/RecordCompletion.md
[Download Certificate of Completion (COC)]: UseCase/DownloadCOC.md
