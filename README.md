# test-results-reporter

Publish test results to Microsoft Teams and Slack

## Getting Started

```sh
npx test-results-reporter publish -c path/to/config.json
```

## Config

Configuration file holds the different configurations files for our reporting needs. We can specify the type of test results to be consumed and type of reports to be published.

### Sample Config File

```json
{
  "reports": [
    {
      "targets": [
        {
          "name": "teams",
          "url": "<teams-incoming-webhook-url>",
          "publish": "test-summary",
          "links": [{ "text": "Build Logs", "url": "<url>" }]
        }
      ],
      "results": [
        {
          "type": "testng",
          "files": ["path/to/testng-results.xml"]
        }
      ]
    }
  ]
}
```

### Environment Variables

To use environment variables in the config file, wrap the environment variable name inside `{}`.

#### Example

```json
{
  "links": [{ "text": "Build Logs", "url": "{BUILD_URL}" }]
}
```

### Sample Reports

![teams-summary-report](https://github.com/test-results-reporter/reporter/raw/main/assets/teams/test-summary-single-suite.png)

![teams-summary-report](https://github.com/test-results-reporter/reporter/raw/main/assets/slack/test-summary-single-suite.png)

### Options

#### reports

| Property | Description                        | Attribute |
|----------|------------------------------------|-----------|
| targets  | list of targets to publish reports | Required  |
| results  | list of results to parse           | Required  |
| options  | common options for all targets     | Optional  |

##### targets

| Property     | Description                    | Attribute         |
|--------------|--------------------------------|-------------------|
| name         | name of the target             | Required/Optional |
| url          | url of the target              | Required/Optional |
| publish      | type of report to publish      | Required/Optional |
| links        | links to be part of the report | Optional          |
| title        | title of the report            | Optional          |
| title_suffix | suffix to add to title         | Optional          |
| path         | path to custom js file         | Optional          |

## Supports

### Targets

| Targets         | Support |
|-----------------|---------|
| Microsoft Teams | ✅       |
| Slack           | ✅       |
| Custom          | ✅       |

Supported Report Types

- test-summary
- failure-summary
- test-summary-slim
- failure-summary-slim
- failure-details
- failure-details-slim

### Results

| Result Type | Support |
|-------------|---------|
| TestNG      | ✅       |
| JUnit       | ✅       |

> Under Active Development

## Examples

### Defaults

1. Identifies the target *(slack or teams)* based on the url.
2. Defaults publish to `test-summary` report.

```json
{
  "reports": [
    {
      "targets": [
        {
          "url": "<slack-incoming-webhook-url>"
        }
      ],
      "results": [
        {
          "type": "testng",
          "files": ["path/to/testng-results.xml"]
        }
      ]
    }
  ]
}
```

### Custom

```json
{
  "reports": [
    {
      "targets": [
        {
          "name": "custom",
          "path": "/relative/path/to/custom/.js"
        }
      ],
      "results": [
        {
          "type": "junit",
          "files": ["path/to/junit-results.xml"]
        }
      ]
    }
  ]
}
```