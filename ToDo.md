# Task list

## Add SCO to course
- CEMS UI
  - Form for `courseId`, `scoFile`
  - Table to list all previously added `scoFile`s
- CEMS Backend
  - Endpoints
    - POST new `scoFile` for `courseId`
    - DELETE `scoFile` for `courseId`
    - GET `scoFile` list with page/filter support
    - GET `scoFile` detail by `courseId`
    - GET enrollments filterable by `courseId` with pagination support
      - this will be used to know if there are existing enrollments for a course before allowing a sco file to be added to it.
  - SCO storage
    - add ENV var for EFS mount location SCO_BASE_PATH
    - add EFS for sco files and mount to SCO_BASE_PATH
    - database table `scoFiles` with `id`, `location`, `courseId`
    - repository to store zip in mounted EFS and insert into db table
- Library to interact with Rustici API
  - Upload `scoFile` and set `courseId`

## Generate Company Proxy Sco Files
- CEMS UI
  - Add Generate button to ScoFileManager
  - Add a timestamp to indicate the last time they where generated for company.
- CEMS Backend
  - Endpoints
    - POST endpoint to start the process of generating proxy sco files, should return a 202 and queue a task as it may take a bit of time to complete.
    - GET endpoint to fetch details of the last generation request.
  - repository and database table to store Rustici destinations
  - Proxy SCO Generation Requests
    - database table for company proxyScoFile generation requests
    - database table to track progress of generation requests (requested, started, complete) with timestamp
    - repository method to add a new proxyScoFile generation request
    - repository method to insert a new status for a specific request (insert only, no update)
  - Proxy SCO storage
    - database table to track proxy zip file location and associate with `companyId`, `courseId`
    - repository method to store zip in mounted EFS and insert it into db table
    - library to iterate though all company catalog courses and create proxy SCO's for those not having any yet.
- Library to interact with Rustici API
  - Create `destination` (used once per external LMS)
  - Create dispatch (`courseId`, `destination`)
  - Download proxyScoFile

## Get course detail and proxy SCO files
- CEMS Backend
  - Endpoints
    - GET company catalog
    - GET course detail (includes link to proxy SCO file if one exists)
    - GET approvals for course by `courseId`
    - GET proxy SCO file by `companyId` and `courseId`

## SCORM Course Enrollment
- CEMS Backend
  - create a location to store Rustici registration links associated with `enrollmentId`'s
  - create Rustici registration
  - get and store registration link
- Library to interact with Rustici API
  - create registration (`enrollmentId`, `learnerId`, `courseId`)
  - get registration link

## Consume SCORM Content UseCase
- CEMS UI
  - Enrollments - Provide a link to Rustici for courses which have a SCO file
  - Once clicked open the link in a new tap just like we do now
  - Ensure no 'take exam' links exist for SCORM courses
- CEMS Backend
  - Endpoints
    - Enrollment Detail - /enrollment/`enrollmentId`
      - userId, companyId, learnerId, courseId, scormLink
      - this is needed so the UI knows where the rustici registration is and if it should hide the take exam option.

## Exam Completion
- CEMS Backend
  - Endpoint to receive xAPI messages from Rustici
  - Library to interpret xAPI messages
    - record exam questions and answers

## View Exam Results
- CEMS UI
  - Transcript
    - for scorm courses we need the "view exam" link to point to the Rustici registration which will re-launch the scorm content and they should be able to see their exam

## Download Certificate of Completion (`COC`)
- Certificate Manager (`CertMan`)
  - define a Completion `DTO` and a way to store them
  - map handlebars placeholders to `DTO` properties
  - convert existing certificate template's into handlebars notation
  - create library to build a `COC` from a `DTO`
  - create an restful api for certificate template `CRUD`
  - create an endpoint to receive the `DTO`
  - create an endpoint to fetch a `COC` by `enrollmentId`
- CEMS Backend
  - create a proxy endpoint in CEMS to fetch a `COC` by `enrollmentId` from `CertMan`
  - CEMS library to build completion `DTO`'s
  - library to push completion `DTO`'s to `CertMan`
  - Add a trigger to create and push the `DTO` on successful course completion (both SCORM and non-SCORM)
  - create a migration to transfer past completion data to `CertMan`