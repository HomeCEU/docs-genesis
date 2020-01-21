# Add SCO to course
Cems admins will be able to upload a `scoFile` and associate it with a `courseId`

## Actors
1. cems admins

## Flow
1. [Course Lookup]
1. select "Add SCO File"
1. Submit

## Alternate Flows
### `courseId` already has a `scoFile`
2. SCO filename will display, can not add/replace it

### Learners are already enrolled in `courseId`
2. "Add SCO File" option will not exist or be disabled

[Course Lookup]: CourseLookup.md
