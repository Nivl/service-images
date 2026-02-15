FROM golang:1.26-alpine

RUN apk update && apk upgrade && \
    apk add --no-cache  curl gcc git musl-dev && \
    # github.com/actions/cache requires a version of tar that supports --posix
    # The one installed by default doesn't support it
    apk add --no-cache tar

# https://github.com/go-task/task/releases
ARG TASK_VERSION=3.48.0
ARG TASK_SHASUM="sha256:f4bfc4eef1b2557b262f3cc0a79976a421885cb7b9e71cfe75568a2ebe4d7ae5"
ADD --checksum=${TASK_SHASUM} https://github.com/go-task/task/releases/download/v${TASK_VERSION}/task_linux_amd64.tar.gz task_linux_amd64.tar.gz
RUN tar xvzf task_linux_amd64.tar.gz && \
    mv task /usr/local/bin/task && \
    rm task_linux_amd64.tar.gz

# https://github.com/golangci/golangci-lint/releases
ARG GOLANG_CI_VERSION=2.9.0
ARG GOLANG_CI_SHASUM="sha256:493aaaca2eba6c8bcef847d92716bbd91bbac4b22cdbb0ab5b6a581b32946091"
ADD --checksum=${GOLANG_CI_SHASUM} https://github.com/golangci/golangci-lint/releases/download/v${GOLANG_CI_VERSION}/golangci-lint-${GOLANG_CI_VERSION}-linux-amd64.tar.gz golangci-lint.tar.gz
RUN tar xvzf golangci-lint.tar.gz --strip-components=1 && \
    mv golangci-lint /usr/local/bin/golangci-lint && \
    rm golangci-lint.tar.gz

# https://github.com/gotestyourself/gotestsum/releases
ARG GOTESTSUM_VERSION=1.13.0
ARG GOTESTSUM_SHASUM="sha256:11ccddeaf708ef228889f9fe2f68291a75b27013ddfc3b18156e094f5f40e8ee"
ADD --checksum=${GOTESTSUM_SHASUM} https://github.com/gotestyourself/gotestsum/releases/download/v${GOTESTSUM_VERSION}/gotestsum_${GOTESTSUM_VERSION}_linux_amd64.tar.gz gotestsum.tar.gz
RUN tar xvzf gotestsum.tar.gz && \
    mv gotestsum /usr/local/bin/gotestsum && \
    rm gotestsum.tar.gz

# https://github.com/daveshanley/vacuum/releases
# Needs to also be updated in api/Taskfile.yml once the CI gets updated
# to the new image.
ARG VACUUM_VERSION=0.23.8
ARG VACUUM_SHASUM="sha256:05952794613c354601a95ceaebcdd0c12fd785240475bd7f9637e9fe0a3b498a"
ADD --checksum=${VACUUM_SHASUM} https://github.com/daveshanley/vacuum/releases/download/v${VACUUM_VERSION}/vacuum_${VACUUM_VERSION}_linux_x86_64.tar.gz vacuum.tar.gz
RUN tar xvzf vacuum.tar.gz && \
    mv vacuum /usr/local/bin/vacuum && \
    rm vacuum.tar.gz

# https://github.com/oapi-codegen/oapi-codegen/releases
# https://github.com/oapi-codegen/oapi-codegen/commits/main/
# The api codegen team has not released a new version in a while,
# but they merge PRs regularly, so we'll go with commit based versioning.
#
# Needs to also be updated in api/Taskfile.yml once the CI gets updated
# to the new image.
ARG OAPI_CODEGEN_COMMIT_HASH="1401fbe26ce7e128e9963786742490ff444e3795"
RUN GOBIN=/usr/local/bin go install github.com/oapi-codegen/oapi-codegen/v2/cmd/oapi-codegen@${OAPI_CODEGEN_COMMIT_HASH}
