# Build stage
FROM golang:alpine AS build-env
# Set the working directory inside the container
WORKDIR /src
# Copy only the necessary files
COPY . .
# Build the Go application
RUN go build -o telnet-server .

# final stage
FROM alpine:latest AS final
# Set the working directory inside the container
WORKDIR /app
# Set environment variables
ENV TELNET_PORT=2323 \
    METRIC_PORT=9000
# Copy the compiled binary from the build stage
COPY --from=build-env /src/telnet-server /app/
# Set the entrypoint
ENTRYPOINT ["./telnet-server"]