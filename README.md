# Golium

This library aims to speed up the generation of acceptance tests with [Cucumber](https://cucumber.io/) and Behaviour-Driven Development (BDD). It is based on [godog](https://github.com/cucumber/godog) which is the official Cucumber BDD framework for Golang.

This library leverages godog with 2 main additions:

- Introduce a context in the step functions. The context is based on the standard Golang context: [context.Context](https://golang.org/pkg/context/). It enables to share information among steps without using global variables.
- Provide common steps for some protocols. The current version provides steps for HTTP and DNS protocols.

This is a very initial version which may suffer big changes in the short-term.

## Configuration

The library has a default configuration. However, these settings can be changed with environment variables.

| Environment Variable | Default | Description |
| -------------------- | ------- | ----------- |
| SUITE | golium | Suite name (for logging purposes) | 
| ENVIRONMENT | local | Name of the environment. Golium reads the environment configuration from the file `${DIR_ENVIRONMENTS}/${ENVIRONMENT}.yml`. This configuration is mandatory and is made available to steps with the function `GetEnvironment()`. |
| GODOG_FORMAT | pretty | Format of the tests output generated by godog. Available formats are: `progress`, `cucumber`, `events`, `junit`, `pretty`. See godog documentation for further details. |
| GODOG_CONCURRENCY | 1 | Concurrency level when running the test suite. See godog documentation for further details. |
| DIR_SCHEMAS | ./schemas | Directory where the JSON schemas are available. These JSON schemas are used by some steps to validate some output (e.g. the body of the HTTP response). |
| DIR_ENVIRONMENTS | ./environments | Directory where the configuration for each environment is available. Each environment must have a yml file in this directory. |
| LOG_DIRECTORY | ./logs | Directory where logs are written. There may be multiple log files. Currently, there is one for tracing the execution of the steps and scenarios (golium.log) and another one to save the HTTP requests and HTTP responses (http.log). |
| LOG_LEVEL | INFO | Log level. Possible values are defined by [logrus](https://github.com/sirupsen/logrus) library. |

## Example

The library includes a complete example with some scenarios for HTTP and DNS protocols in the directory `test/acceptance`.

For testing:

```
cd test/acceptance
go test -v
```

The examples contains the following directories:

- `features`. Features for the test suite in BDD.
- `environments`. It contains the configuration for each environment in a specific yml file. This directory is configured with the environment variable: `DIR_ENVIRONMENTS`.
- `schemas`. JSON schemas. This is used by some steps to validate an input (e.g. the HTTP response body). This directory is configured with the environment variable: `DIR_SCHEMAS`.
- `logs`. It stores the log files generated by the execution of the suite tests.

## License

```
Copyright 2021 Telefonica Cybersecurity & Cloud Tech SL

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or lied.
See the License for the specific language governing permissions and
limitations under the License.
```