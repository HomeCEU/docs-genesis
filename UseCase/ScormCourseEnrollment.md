# SCORM Course Enrollment

## Actors
1. learner (self enrollment)
2. manager (assigns course)

## Flow
### Self Enrollment
- Learner login
- Open course catalog
- Select course and enroll

### Manual Assignment
- manager login
- Open manual course assignment
- Select learner and course
- Submit

### Automatic Assignment
- learner matches a curriculum filter
- system assigns course to learner

### 3rd Party LMS assignment
- ... ????

## TODO
- Library to interact with Rustici API
  - create registration (`enrollmentId`, `learnerId`, `courseId`)
  - get registration link
- CEMS Backend
  - create a location to store Rustici registration links associated with `enrollmentId`'s
  - create Rustici registration
  - get and store registration link
