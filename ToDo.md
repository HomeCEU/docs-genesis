# Task list

## Course Lookup
- CEMS UI
- CEMS Backend
  - Endpoints
    - Course Detail
      - must include total number of enrollments
      - must include scormFileName

## Add SCO to course
- CEMS UI
  - Form for `courseId`, `scoFile` [CEMS-1645], [CEMS-1660], [CEMS-1662], [CEMS-1663], [CEMS-1664]
- CEMS Backend
  - Endpoints
    - POST new `scoFile` for `courseId` [CEMS-1641]
    - GET `scoFile` detail by `courseId`
  - SCO storage
    - add EFS for sco files and mount to SCO_BASE_PATH ✔️ [CEMS-1637]
    - database table `scoFiles` with `id`, `location`, `courseId` ✔️ [CEMS-1638]
    - repository to store zip in mounted EFS and insert into db table ✔️ [CEMS-1639]
- Library to interact with Rustici API
  - Upload `scoFile` and set `courseId` [CEMS-1640]

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
    - database table to track proxy zip file location and associate with `companyId`, `courseId` [CEMS-1676]
    - repository method to store zip in mounted EFS and insert it into db table [CEMS-1677]
        - library to create proxy SCO's . [CEMS-1675]
    - library to iterate though all company catalog courses and create proxy SCO's for those not having any yet. [CEMS-1678]
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

## Consume SCORM Content
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
  - Templates
    - convert existing certificate template's into handlebars notation [CEMS-1672]
    - map handlebars placeholders to `DTO` properties [CEMS-1622]
    - create template type's
      - certificate
      - disiplineBrandBlock
      - image
    - Endpoints
      - PUT /template/`typeConstant`/`constant` - json(name, author, body) [CEMS-1620]
      - GET /template/`typeConstant`/`constant` - json(name, author, updatedAt, bodyURI) [CEMS-1618]
      - GET /template/`typeConstant`/`constant`/body - plain-text(body) [CEMS-1618]
      - GET /template?filter[type]=`typeConstant` - list of templates filterable by type [CEMS-1680]
  - Certificate & DataTransferObject `DTO`
    - define a Completion `DTO` and a way to store them [CEMS-1569]
    - create library to build a `COC` from a `DTO` [CEMS-1608]
    - Endpoints
      - PUT /entity/`entityId` - json
      - GET /entity/`entityId` - json
      - GET /render/`entityId`?type=`typeConstant`&template=`constant`&format=pdf|html
- CEMS Backend
  - DataTransferObject `DTO`
    - CEMS library to build completion `DTO`'s [CEMS-1650]
    - library to push completion `DTO`'s to `CertMan`
    - Add a trigger to create and push the `DTO` on successful course completion (both SCORM and non-SCORM)
  - Data Migration
    - create a migration to transfer past completion data to `CertMan`
  - Endpoint
    - GET /completion/certificate?enrollmentId=`enrollmentId` - proxy endpoint in CEMS to fetch a `COC` pdf from `CertMan`

## Phase 2
- CEMS Backend
  - Extract exam answer text from sco files [CEMS-1651]
- WYSIWYG template builder

[CEMS-1569]: https://homeceu.atlassian.net/browse/CEMS-1569
[CEMS-1608]: https://homeceu.atlassian.net/browse/CEMS-1608
[CEMS-1618]: https://homeceu.atlassian.net/browse/CEMS-1618
[CEMS-1620]: https://homeceu.atlassian.net/browse/CEMS-1620
[CEMS-1622]: https://homeceu.atlassian.net/browse/CEMS-1622
[CEMS-1637]: https://homeceu.atlassian.net/browse/CEMS-1637
[CEMS-1638]: https://homeceu.atlassian.net/browse/CEMS-1638
[CEMS-1639]: https://homeceu.atlassian.net/browse/CEMS-1639
[CEMS-1640]: https://homeceu.atlassian.net/browse/CEMS-1640
[CEMS-1641]: https://homeceu.atlassian.net/browse/CEMS-1641
[CEMS-1645]: https://homeceu.atlassian.net/browse/CEMS-1645
[CEMS-1650]: https://homeceu.atlassian.net/browse/CEMS-1650
[CEMS-1651]: https://homeceu.atlassian.net/browse/CEMS-1651
[CEMS-1660]: https://homeceu.atlassian.net/browse/CEMS-1660
[CEMS-1662]: https://homeceu.atlassian.net/browse/CEMS-1662
[CEMS-1663]: https://homeceu.atlassian.net/browse/CEMS-1663
[CEMS-1664]: https://homeceu.atlassian.net/browse/CEMS-1664
[CEMS-1672]: https://homeceu.atlassian.net/browse/CEMS-1672
[CEMS-1675]: https://homeceu.atlassian.net/browse/CEMS-1675
[CEMS-1676]: https://homeceu.atlassian.net/browse/CEMS-1676
[CEMS-1677]: https://homeceu.atlassian.net/browse/CEMS-1677
[CEMS-1678]: https://homeceu.atlassian.net/browse/CEMS-1678
[CEMS-1680]: https://homeceu.atlassian.net/browse/CEMS-1680
