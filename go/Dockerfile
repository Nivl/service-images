FROM golang:1.24-alpine

RUN apk update && apk upgrade
RUN apk add curl git gcc musl-dev
# github.com/actions/cache requires a version of tar that supports --posix
# The one installed by default doesn't support it
RUN apk add --no-cache tar

# https://github.com/go-task/task/releases
ARG TASK_VERSION=3.44.1
ARG TASK_SHASUM="62969d22bee5ea8950cc64a30cc7eb278f89ad67683132e728841457aae523c1  task_linux_amd64.tar.gz"
RUN curl -L -o task_linux_amd64.tar.gz https://github.com/go-task/task/releases/download/v${TASK_VERSION}/task_linux_amd64.tar.gz && \
    tar xvzf task_linux_amd64.tar.gz && \
    echo "$TASK_SHASUM" | sha256sum -c - && \
    mv task /usr/local/bin/task && \
    rm task_linux_amd64.tar.gz


# https://github.com/golangci/golangci-lint/releases
ARG GOLANG_CI_VERSION=2.3.0
ARG GOLANG_CI_SHASUM="b15f994fe7b9885e3862d394e159025fc2ca808d0dccf2ce3d6a242896c2be0c  golangci-lint.tar.gz"
RUN curl -L -o golangci-lint.tar.gz https://github.com/golangci/golangci-lint/releases/download/v${GOLANG_CI_VERSION}/golangci-lint-${GOLANG_CI_VERSION}-linux-amd64.tar.gz && \
    tar xvzf golangci-lint.tar.gz --strip-components=1 && \
    echo "$GOLANG_CI_SHASUM" | sha256sum -c - && \
    mv golangci-lint /usr/local/bin/golangci-lint && \
    rm golangci-lint.tar.gz


# https://github.com/gotestyourself/gotestsum/releases
ARG GOTESTSUM_VERSION=1.12.3
ARG GOTESTSUM_SHASUM="dd644e817b509fa8e216887ec93facdd85722d1b78f6a19149ee1bc9f59fca64  gotestsum.tar.gz"
RUN curl -L -o gotestsum.tar.gz https://github.com/gotestyourself/gotestsum/releases/download/v${GOTESTSUM_VERSION}/gotestsum_${GOTESTSUM_VERSION}_linux_amd64.tar.gz && \
    tar xvzf gotestsum.tar.gz && \
    echo "$GOTESTSUM_SHASUM" | sha256sum -c - && \
    mv gotestsum /usr/local/bin/gotestsum && \
    rm gotestsum.tar.gz


# https://github.com/daveshanley/vacuum/releases
# Needs to also be updated in api/Taskfile.yml once the CI gets updated
# to the new image.
ARG VACUUM_VERSION=0.17.6
ARG VACUUM_SHASUM="9c0e3fed6841abaf1a9bd4ec146ba7a8e03221806598839a4ad75a81d9a346c9  vacuum.tar.gz"
RUN curl -L -o vacuum.tar.gz https://github.com/daveshanley/vacuum/releases/download/v${VACUUM_VERSION}/vacuum_${VACUUM_VERSION}_linux_x86_64.tar.gz && \
    tar xvzf vacuum.tar.gz && \
    echo "$VACUUM_SHASUM" | sha256sum -c - && \
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
