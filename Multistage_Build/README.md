# Docker Multi-Stage Build

* A multi-stage build in Docker allows you to use multiple FROM statements in a single Dockerfile, building your application in stages.

* It is primarily used to reduce the final image size by separating build-time dependencies from the runtime environment.

* If we have multiple FROM in a dockerfile it will give number to the FROM, like for 1st FROM it will give 0 (zero), for second FROM it will give 1 and so on.

* Instead of number we can also give alias name for each build at FROM directive and make use of that name while copying a build or content to the next build.

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Runtime
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

* Stage 1 – Build stage

  * Use a full-featured image (golang:1.21)

  * Compile the application (go build)

  * Label stage as builder using AS builder.

* Stage 2 – Final stage

  * Use a minimal image (alpine)

  * Copy only the compiled binary from the previous stage

    ```cmd
    COPY --from=builder /app/myapp .
    ```

* The final image does not contain build tools or source code, only runtime dependencies.

* To build an image from the scratch we haveto use "scratch"

  ```dockerfile
  FROM scratch
  ```