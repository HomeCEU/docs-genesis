# Download Certificate of Completion (COC)

## Terms
- `COC` - Certificate Of Completion
- `DTO` - Data Transfer Object
- `CT` - Certificate Template
- `CRUD` - create, read, update, delete
- `CertMan` - Certificate Manager

## Actors
1. learner

## Flow
1. Learner completes any course with a passing grade
1. Completion data is sent to `CertMan`
1. Learner clicks to Download their `COC`

## ToDo
- Certificate Manager  - `CertMan`
  - define a Completion `DTO` and a way to store them
  - map handlebars placeholders to `DTO` properties
  - convert existing `CT`'s into handlebars notation
  - create library to build a `COC` from a `DTO`
  - create an restful api for `CT` `CRUD`
  - create an endpoint to receive the `DTO`
  - create an endpoint to fetch a `COC` by `enrollmentId`
- CEMS Backend
  - create a proxy endpoint in CEMS to fetch a `COC` by `enrollmentId` from `CertMan`
  - modify CEMS to build the `DTO`'s and push them to `CertMan`
  - create a migration to transfer past completion data to `CertMan`
