FROM golang:1.25-alpine

RUN apk update && apk upgrade && \
    apk add --no-cache  curl gcc git musl-dev && \
    # github.com/actions/cache requires a version of tar that supports --posix
    # The one installed by default doesn't support it
    apk add --no-cache tar

# https://github.com/go-task/task/releases
ARG TASK_VERSION=3.44.1
ARG TASK_SHASUM="62969d22bee5ea8950cc64a30cc7eb278f89ad67683132e728841457aae523c1"
ADD --checksum=sha256:${TASK_SHASUM} https://github.com/go-task/task/releases/download/v${TASK_VERSION}/task_linux_amd64.tar.gz task_linux_amd64.tar.gz
RUN tar xvzf task_linux_amd64.tar.gz && \
    mv task /usr/local/bin/task && \
    rm task_linux_amd64.tar.gz

# https://github.com/golangci/golangci-lint/releases
ARG GOLANG_CI_VERSION=2.4.0
ARG GOLANG_CI_SHASUM="fae792524c04424c0ac369f5b8076f04b45cf29fc945a370e55d369a8dc11840"
ADD --checksum=sha256:${GOLANG_CI_SHASUM} https://github.com/golangci/golangci-lint/releases/download/v${GOLANG_CI_VERSION}/golangci-lint-${GOLANG_CI_VERSION}-linux-amd64.tar.gz golangci-lint.tar.gz
RUN tar xvzf golangci-lint.tar.gz --strip-components=1 && \
    mv golangci-lint /usr/local/bin/golangci-lint && \
    rm golangci-lint.tar.gz

# https://github.com/gotestyourself/gotestsum/releases
ARG GOTESTSUM_VERSION=1.12.3
ARG GOTESTSUM_SHASUM="dd644e817b509fa8e216887ec93facdd85722d1b78f6a19149ee1bc9f59fca64"
ADD --checksum=sha256:${GOTESTSUM_SHASUM} https://github.com/gotestyourself/gotestsum/releases/download/v${GOTESTSUM_VERSION}/gotestsum_${GOTESTSUM_VERSION}_linux_amd64.tar.gz gotestsum.tar.gz
RUN tar xvzf gotestsum.tar.gz && \
    mv gotestsum /usr/local/bin/gotestsum && \
    rm gotestsum.tar.gz

# https://github.com/daveshanley/vacuum/releases
# Needs to also be updated in api/Taskfile.yml once the CI gets updated
# to the new image.
ARG VACUUM_VERSION=0.17.8
ARG VACUUM_SHASUM="96415f3bd5d9380846a5ee8821538e45ee45addbcf0ec734c24d3b9a4daf5ed1"
ADD --checksum=sha256:${VACUUM_SHASUM} https://github.com/daveshanley/vacuum/releases/download/v${VACUUM_VERSION}/vacuum_${VACUUM_VERSION}_linux_x86_64.tar.gz vacuum.tar.gz
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
ARG OAPI_CODEGEN_COMMIT_HASH="702cc9178842ef30e8ea8b1ec3aeb9637fa3895f"
RUN GOBIN=/usr/local/bin go install github.com/oapi-codegen/oapi-codegen/v2/cmd/oapi-codegen@${OAPI_CODEGEN_COMMIT_HASH}

# https://github.com/uber-go/mock/releases
# https://github.com/uber-go/mock/commits/main/
# They don't release binaries
# v0.6.0
ARG GOMOCK_COMMIT_HASH="2d1c58167e30f380cf78e44a43b100a14767e817"
RUN GOBIN=/usr/local/bin go install go.uber.org/mock/mockgen@${GOMOCK_COMMIT_HASH}
