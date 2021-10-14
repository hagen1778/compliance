FROM golang:1.16 as build-env

WORKDIR /go/src/promql
COPY . /go/src/promql

RUN go build ./cmd/promql-compliance-tester

FROM quay.io/prometheus/busybox
COPY --from=build-env /go/src/promql/promql-compliance-tester /
COPY --from=build-env /go/src/promql/promql-compliance-cases.yml /
COPY --from=build-env /go/src/promql/promql-compliance-tester.yml /

ENTRYPOINT ["/promql-compliance-tester"]