# PromQL Compliance Tester

The PromQL Compliance Tester is a tool for running comparison tests between native Prometheus and vendor PromQL API implementations.

The tool was [first published and described](https://promlabs.com/blog/2020/08/06/comparing-promql-correctness-across-vendors) in August 2020. [Test results have been published](https://promlabs.com/promql-compliance-tests) on 2020-08-06 and 2020-12-01.

## Building via Docker

If you have docker installed, you can build the tool using docker.

```bash
 docker build . -t promql-compliance-tester:latest
```

## Build from source

### Requirements

This tool is written in Go and requires a working Go setup to build. Library dependencies are handled via [Go Modules](https://blog.golang.org/using-go-modules).

### Building

To build the tool:

```bash
go build ./cmd/promql-compliance-tester
```

## Executing

Tool allows setting following flags:

```
$ ./promql-compliance-tester -h
Usage of /promql-compliance-tester:
  -config-file string
    	The path to the configuration file. (default "promql-compliance-tester.yml")
  -output-format string
    	The comparison output format. Valid values: [text, html, json] (default "text")
  -output-html-template string
    	The HTML template to use when using HTML as the output format. (default "./output/example-output.html")
  -output-passing
    	Whether to also include passing test cases in the output.
  -queries-file string
    	The path to the test cases to run. (default "promql-compliance-cases.yml")
  -query-parallelism int
    	Maximum number of comparison queries to run in parallel. (default 20)
```

Running the tool will execute all test cases in `-queries-file` and compare results between reference and target provided in `-config-file`.

At the end of run, the output is provided in form or number of executed tests and errors if any. Example output can be seen here:

```bash
./promql-compliance-tester -config-file config.yaml
529 / 529 [-----------------------------------------------------------------------------------------------------------] 100.00% 278 p/s
Total: 529 / 529 (100.00%) passed, 0 unsupported
```

If all tests were executed correctly and passing the tool returns 0 code, otherwise it returns 1.

## Configuration

The query tweaks, and PromQL API endpoints to use are specified in a configuration file using `config-file` flag. An example configuration file with settings for Thanos, Cortex, TimescaleDB, and VictoriaMetrics is included [here](promql-compliance-tester.yml).

The test cases are configured using `queries-file` option. An example cases used in recent compliance tests are provided [here](promql-compliance-cases.yml)

## Contributing

Help wanted to improve the PromQL Compliance Tester. In particular, we would love to add and improve the following points:

* Test instant queries in addition to range queries.
* Add more variation and configurability to input timestamps.
* Flesh out a more comprehensive (and less overlapping) set of input test queries.
* Automate and integrate data loading into different systems.
* Test more vendor implementations of PromQL.
* Version test results and make pretty output presentations easier.

**Note:** Many people will be interested in benchmarking performance differences between PromQL implementations. While this is important as well, the PromQL Compliance Tester focuses solely on correctness testing. Please contact the [maintainers](../MAINTAINERS.md) if you want to work on performance testing.

If you would like to help flesh out the tester, please [file issues](https://github.com/prometheus/compliance/issues) or [pull requests](https://github.com/prometheus/compliance/pulls).
