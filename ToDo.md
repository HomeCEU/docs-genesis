# Task list

## Course Lookup
- CEMS UI
  - User Interface to lookup courses (auto filter by video only!) **hector**
  - Actions
    - Generate Proxy files for company **hector**
    - Add Sco file for course **hector**
    - Duplicate course so we can add sco files [CEMS-1698]
- CEMS Backend
  - Endpoints
    - GET ​/company​/{companyId}​/course​/{courseId} [CEMS-1671]
    - POST /course/{courseId}/duplicate [CEMS-1697]
  - Duplicate course so we can add sco files [CEMS-1696]
    - maintain some reference back to original course so that we can replace courses in the company catalog in the future, not in this task
    - fields to copy
      - name
      - description
      - boc instructional level
      - instructional level
      - clinical topics
      - is_active
      - course_format
      - hours
      - outlinie
      - summary
      - goals
      - preview_name
      - preview_link
      - preview_type
      - author_summary
      - displines
      - categories
      - authors
      - asha_code
      - all accredidations
      - owner
      - mail_price
      - online_price
      - royalty_owner
      - notes

## Add SCO to course
- CEMS UI
  - Course Lookup
    - display current sco detail or provide action to upload file. **hector**
- CEMS Backend
  - Endpoints
    - POST new `scoFile` for `courseId` ✔️ [CEMS-1641]
  - SCO storage
    - add EFS for sco files and mount to SCO_BASE_PATH ✔️ [CEMS-1637]
    - database table `scoFiles` with `id`, `location`, `courseId` ✔️ [CEMS-1638]
    - repository to store zip in mounted EFS and insert into db table ✔️ [CEMS-1639]
  - create empty inactive exam for course because course can not be activiated without it [CEMS-1705]
    - use 'SCORM' as name
  - Buil import sco files [CEMS-1699]
    - Given a bunch of zip files named {courseId}.zip
    - For each zip file
      - duplicate courseId
      - add sco to new course (local + rustici)
- Library to interact with Rustici API
  - Upload `scoFile` and set `courseId` ✔️ [CEMS-1640]

## Generate Company Proxy Sco Files
- CEMS UI
  - Add Generate button to CourseLookup **hector**
- CEMS Backend
  - Endpoints
    - POST endpoint to start the process of generating proxy sco files, should return a 202 and queue a task as it may take a bit of time to complete. [CEMS-1700]
    - GET endpoint to fetch details of the last generation request. **Phase2+**
  - ~~repository and database table to store Rustici destinations~~
  - Proxy SCO Generation Requests
    - database table for company proxyScoFile generation requests [CEMS-1706]
    - database table to track progress of generation requests (requested, started, complete) with timestamp [CEMS-1707]
    - repository method to add a new proxyScoFile generation request [CEMS-1708]
    - repository method to insert a new status for a specific request (insert only, no update) [CEMS-1709]
  - Proxy SCO storage
    - database table to track proxy zip file location and associate with `companyId`, `courseId` ✔️  [CEMS-1676]
    - repository method to store zip in mounted EFS and insert it into db table ✔️  [CEMS-1677]
        - library to create proxy SCO's. ✔️ [CEMS-1675]
    - library to iterate though all company catalog courses and create proxy SCO's for those not having any yet. [CEMS-1678]
- Library to interact with Rustici API [CEMS-1675]
  - ~~Create `destination` (used once per external LMS)~~
  - Create dispatch (`courseId`, `destination`)
  - Download proxyScoFile

## Get course detail and proxy SCO files
- CEMS Backend
  - Endpoints
    - GET company catalog ✔️ 
      - Already existed as GET
​/company​/{companyId}​/courses
    - GET course detail (includes link to proxy SCO file if one exists) [CEMS-1671]
      - GET ​/company​/{companyId}​/course​/{courseId}
    - GET approvals for course by `courseId` [CEMS-1701]
      - GET​/accreditations?filter[courseId]={courseId}

## SCORM Course Enrollment
- CEMS Backend
  - create a location to store Rustici registration links associated with `enrollmentId`'s [CEMS-1710]
  - create Rustici registration [CEMS-1711]
  - get and store registration link [CEMS-1712]
- Library to interact with Rustici API **Carlos**
  - create registration (`enrollmentId`, `learnerId`, `courseId`) [[CEMS-1713]
  - get registration link [CEMS-1714]

## Consume SCORM Content
- CEMS UI  **Phase2**
  - Enrollments - Provide a link to Rustici for courses which have a SCO file
  - Once clicked open the link in a new tap just like we do now
  - Ensure no 'take exam' links exist for SCORM courses
- CEMS Backend
  - Endpoints
    - Enrollment Detail - /enrollment/`enrollmentId` [CEMS-1703]
      - userId, companyId, learnerId, courseId, courseName, scormLink
      - certificate link (if completed)
      - this is needed so the UI knows where the rustici registration is and if it should hide the take exam option.
    - POST
​/enrollment​/{enrollmentId}​/reportToAsha [CEMS-1704]
      - way for external lms users to request that we report their completion to asha

## Exam Completion
- CEMS Backend **Carlos**
  - Endpoint to receive xAPI messages from Rustici [CEMS-1715]
  - Library to interpret xAPI messages [CEMS-1716]
    - record exam questions and answers [CEMS-1717]

## View Exam Results
- CEMS UI
  - Transcript **Phase2**
    - for scorm courses we need the "view exam" link to point to the Rustici registration which will re-launch the scorm content and they should be able to see their exam

# Certificates
## TemplateManager
- Database
  - table `template`
    - columns
      - templateId   - uuid
      - templateType - used to filter templates by a type/category
      - templateKey  - user provided key
      - author       - user provided string for author name
      - createdAt    - UTC datetime
      - body         - MEDIUMTEXT
    - index
      - PK (templateId)
      - index (templateType, templateKey)
  - table `entity`
    - columns
      - entityId   - uuid
      - entityType - used as a category 
      - entityKey  - unique
      - createdAt  - UTC datetime
      - data       - json data structure
    - index
      - PK (entityId)
      - index (entityType, entityKey)
  - table `renderLog`
    - columns
      - requestId  - int
      - request    - json request params
      - templateId - uuid
      - entityId   - uuid
      - createdAt  - UTC datetime
      - format     - pdf/html
    - index
      - PK (requestId)
      - FK entityId
      - FK tempkateId
- Lib
  - Entity
    - entityId
    - entityType
    - entityKey
    - data
    - createdAt
  - EntityRepository
    - save(type, key, Entity)    void
    - find(type, key)            array of {entityId, entityType, entityKey, createdAt}
  - Document
    - pdf(templateId, entityId)  string filePath

- Endpoints
  - POST /entity
    - Request JSON
      - entityType    string
      - entityKey     string
      - data          object
    - Response JSON
      - entityId      uuid
      - entityType    string
      - entityKey     string
      - createdAt     ISO 8601 date-time
  - GET  /entity/{entityType}/{entityKey}/history
    - Respose JSON
      - total         int
      - items (array of)
        - entityId    uuid
        - entityType  string
        - entityKey   string
        - createdAt   ISO 8601 date-time
  - GET  /render
    - Request Query
      - templateId    uuid
      - templateType  string
      - templateKey   string
      - entityId      uuid
      - entityType    string
      - entityKey     string
    - Response
      - entityId.pdf  application/pdf


## CEMS - Download Certificate of Completion (CoC)
  - Lib
    - CertMan
      - hasCompletionData(eid)
        - count(entity version history) > 0
      - recordCompletion(eid, completionDTO)
      - buildCertificate(template, eid)
    - CompletionDto
      - buildFromEnrollment(eid)
  - Endpoint
    - GET /enrollment/{eid}/certificate
      - certMan::hasCompletion(eid)
        - else
          - dto=CompletionDto::buildFromEnrollment(eid)
          - certMan::recordCompletion(eid, dto)
      - certMan::render(templateKey, eid)

## Download Certificate of Completion (`COC`)
- Certificate Manager (`CertMan`)
  - Templates
    - convert existing certificate template's into handlebars notation [CEMS-1672]
    - map handlebars placeholders to `DTO` properties ✔️  [CEMS-1622]
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
      - PUT /entity/`entityId` - json [CEMS-1686]
      - GET /entity/`entityId` - json [CEMS-1687]
      - GET /render/`entityId`?type=`typeConstant`&template=`constant`&format=pdf|html [CEMS-1688]
- CEMS Backend
  - DataTransferObject `DTO`
    - CEMS library to build completion `DTO`'s [CEMS-1650]
    - library to push completion `DTO`'s to `CertMan` [CEMS-1689]
    - Add a trigger to create and push the `DTO` on successful course completion (both SCORM and non-SCORM) [CEMS-1690]
  - Data Migration
    - create a migration to transfer past completion data to `CertMan` [CEMS-1691]
  - Endpoint
    - GET /completion/certificate?enrollmentId=`enrollmentId` - proxy endpoint in CEMS to fetch a `COC` pdf from `CertMan` [CEMS-1692]


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
[CEMS-1671]: https://homeceu.atlassian.net/browse/CEMS-1671
[CEMS-1672]: https://homeceu.atlassian.net/browse/CEMS-1672
[CEMS-1675]: https://homeceu.atlassian.net/browse/CEMS-1675
[CEMS-1676]: https://homeceu.atlassian.net/browse/CEMS-1676
[CEMS-1677]: https://homeceu.atlassian.net/browse/CEMS-1677
[CEMS-1678]: https://homeceu.atlassian.net/browse/CEMS-1678
[CEMS-1680]: https://homeceu.atlassian.net/browse/CEMS-1680
[CEMS-1686]: https://homeceu.atlassian.net/browse/CEMS-1686
[CEMS-1687]: https://homeceu.atlassian.net/browse/CEMS-1687
[CEMS-1688]: https://homeceu.atlassian.net/browse/CEMS-1688
[CEMS-1689]: https://homeceu.atlassian.net/browse/CEMS-1689
[CEMS-1690]: https://homeceu.atlassian.net/browse/CEMS-1690
[CEMS-1691]: https://homeceu.atlassian.net/browse/CEMS-1691
[CEMS-1692]: https://homeceu.atlassian.net/browse/CEMS-1692
[CEMS-1696]: https://homeceu.atlassian.net/browse/CEMS-1696
[CEMS-1697]: https://homeceu.atlassian.net/browse/CEMS-1697
[CEMS-1698]: https://homeceu.atlassian.net/browse/CEMS-1698
[CEMS-1699]: https://homeceu.atlassian.net/browse/CEMS-1699
[CEMS-1700]: https://homeceu.atlassian.net/browse/CEMS-1700
[CEMS-1701]: https://homeceu.atlassian.net/browse/CEMS-1701
[CEMS-1703]: https://homeceu.atlassian.net/browse/CEMS-1703
[CEMS-1704]: https://homeceu.atlassian.net/browse/CEMS-1704
[CEMS-1705]: https://homeceu.atlassian.net/browse/CEMS-1705
[CEMS-1706]: https://homeceu.atlassian.net/browse/CEMS-1706
[CEMS-1707]: https://homeceu.atlassian.net/browse/CEMS-1707
[CEMS-1708]: https://homeceu.atlassian.net/browse/CEMS-1708
[CEMS-1709]: https://homeceu.atlassian.net/browse/CEMS-1709
[CEMS-1710]: https://homeceu.atlassian.net/browse/CEMS-1710
[CEMS-1711]: https://homeceu.atlassian.net/browse/CEMS-1711
[CEMS-1712]: https://homeceu.atlassian.net/browse/CEMS-1712
[CEMS-1713]: https://homeceu.atlassian.net/browse/CEMS-1713
[CEMS-1714]: https://homeceu.atlassian.net/browse/CEMS-1714
[CEMS-1715]: https://homeceu.atlassian.net/browse/CEMS-1715
[CEMS-1716]: https://homeceu.atlassian.net/browse/CEMS-1716
[CEMS-1717]: https://homeceu.atlassian.net/browse/CEMS-1717
