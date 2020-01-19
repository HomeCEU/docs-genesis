# Generate Company Proxy Sco Files

Companies using an external LMS will require proxy sco files.

The system will not know which companies need them and which don't so for now there will be a user initiated action which will ensure we have proxy sco files for all scorm courses within a given company.

## Actors
1. cems admin

## Flow
1. Login
1. Open Sco file manager within company requiring proxy files
1. Click Generate Proxy Files for Company

## ToDo
- CEMS UI
  - Add Generate button
- CEMS Backend
  - Endpoints
    - POST endpoint to start the process of generating proxy sco files, should return a 202 and queue a task as it may take a bit of time to complete.
  - Proxy SCO storage
    - database table to track proxy zip file location and associate with `companyId`, `courseId`
    - repository to store zip in mounted EFS and insert it into db table
    - library to iterate though all company catalog courses and create proxy sco's for those not having any yet.
- Library to interact with Rustici API
  - Create `destination` (used once per external LMS)
  - Create dispatch (`courseId`, `destination`)
