# Add SCO to course
Cems admins will be able to upload a `scoFile` and associate it with a `courseId`

## Actors
1. cems admins

## Flow
1. Login
1. Open Sco file manager
1. Select 'New'
1. Input `courseId`
1. select `scoFile`
1. Submit

## Alternate Flows
### `courseId` already has a `scoFile`
5. An error will display, Rustici does not permit this.
1. GoTo 4

### `courseId` does not exist
5. An error will display
1. GoTo 4

### Learners are already enrolled in `courseId`
5. An error will display
1. GoTo 4
