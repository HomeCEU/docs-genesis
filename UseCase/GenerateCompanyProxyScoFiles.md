# Generate Company Proxy Sco Files

Companies using an external LMS will require proxy sco files.

The system will not know which companies need them and which don't so for now there will be a user initiated action which will ensure we have proxy sco files for all SCORM courses for a given company.

This action would check every course in the company catalog, and for those which have a sco file but not a proxy file it will create a proxy file for them.

## Actors
1. cems admin

## Flow
1. [Course Lookup]
1. See "Generate Proxy Files for Company"
1. select company
1. click "Generate"

[Course Lookup]: CourseLookup.md
