# Add SCO to course
Cems admins will be able to upload a `scoFile` and associate it with a `courseId`

## Actors
1. cems admins

## Flow
1. Login
1. Open Sco file manager
1. Lookup Course
1. select "Add SCO File"
1. Submit

## Alternate Flows
### `courseId` already has a `scoFile`
4. SCO filename and timestamp will display, use can not add/replace it

### `courseId` does not exist
4. An error will display
1. GoTo 4

### Learners are already enrolled in `courseId`
4. "Add SCO File" option will not exist or be disabled
5. the number of enrollments will be displayed.
