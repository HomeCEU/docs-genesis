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
1. Submit

### `courseId` does not exist
5. An error will display
1. GoTo 4

## ToDo
- CEMS UI
  - Form for `courseId`, `scoFile`
  - Table to list all previously added `scoFile`s
- CEMS Backend
  - Endpoints
    - POST new `scoFile` for `courseId`
    - DELETE `scoFile` for `courseId`
    - GET `scoFile` list with page/filter support
    - GET `scoFile` detail by `courseId`
  - SCO storage
    - add ENV var for EFS mount location SCO_BASE_PATH
    - add EFS for sco files and mount to SCO_BASE_PATH
    - database table `scoFiles` with `id`, `location`, `courseId`
    - repository to store zip in mounted EFS and insert into db table
- Library to interact with Rustici API
  - Upload `scoFile` and set `courseId`
