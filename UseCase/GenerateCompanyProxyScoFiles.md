# Generate Company Proxy Sco Files

Companies using an external LMS will require proxy sco files.

The system will not know which companies need them and which don't so for now there will be a user initiated action which will ensure we have proxy sco files for all SCORM courses for a given company.

This action would check every course in the company catalog, and for those which have a sco file but not a proxy file it will create a proxy file for them.

## Actors
1. cems admin

## Flow
1. Login
1. Open Sco file manager within company requiring proxy files
1. Click Generate Proxy Files for Company

## ToDo
- Library to interact with Rustici API
  - Create `destination` (used once per external LMS)
  - Create dispatch (`courseId`, `destination`)
  - Download proxyScoFile
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
    - library to iterate though all company catalog courses and create proxy sco's for those not having any yet.
