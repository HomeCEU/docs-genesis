# Get course detail and proxy SCO files
Given a company using another LMS they will need to be able to access their course catalog though our restful API

This includes

- course detail (id, name, outline, etc.)
- approvals (start date, end date, state, discipline, etc.)
- link to proxy sco file created specifically for their company.

## Actors
There are no human actors in this UseCase, it will need to be automated.

### Non-Human Actors
- CEMS Backend `CEMS`
- External LMS `ELMS`

## Flow
1. `ELMS` consumes course catalog from `CEMS` API
1. `ELMS` downloads proxy SCO files from links provided
