# Multi Stage Builds

- Came out in 2017
- Multiple FROM
- COPY files between stages
- Size benefits, smaller output
- Lets the actual image be the artifact of the smallest thing the program needs,
  not all the tooling

Separate test stage
