# sns-subman
Subscription Manager for AWS SNS.

## Why?

(Automatic) managing SNS subscriptions is pretty cumbersome. Either it involves a lot of commands or a lot of boilerplat code in order to create resources, subscriptions and permissions. The solution - you guessed it: `sns-subman`.

## Installation

To install the latest version locally, you can run the following command

`curl https://raw.githubusercontent.com/moee/sns-subman/master/sns-subman > /usr/local/bin/sns-subman && chmod +x /usr/local/bin/sns-subman`

## Credentials

This tool uses boto3. Please refer to the [Credentials Configuration](http://boto3.readthedocs.io/en/latest/guide/configuration.html) section in the user manual.

## Usage

```
usage: sns-subman [-h] [--dry-run] [--version] config

Manage SNS subscriptions

positional arguments:
  config      The file that contains the topic and subscription informtion

optional arguments:
  -h, --help  show this help message and exit
  --dry-run   Don't do anything, only show what would happen.
  --version   show program's version number and exit
```

## Config File

The topics and subscriptions are managed by a config file. `sns-subman` will automatically create all topics, queues and subscriptions and handle permissions based on this config file.

## Hands-on Example

In order to create a topic named `hello-world-topic` and subscribe a queue `hello-world-queue` to it you can use the following config file.

```javascript
{
    "subscriptions" : {
		"hello-world-topic" : [
			{ "sqs" : "hello-world-queue" }
		]
	}
}
```

To create the topic, queue and subscription simply run

`./sns-subman <CONFIG_FILE>`

### Config file format

The config file is in the following format

```
{
	"endpoints" : {
		"sns" : <SNS-ENDPOINT>,
		"sqs" : <SQS-ENDPOINT>
	},
	"subscriptions" : {
		"<TOPIC-NAME>": [
			{ "<PROTOCOL>" : "<ENDPOINT>" },
			...
		],
		...
		}
	}
}
```

* `endpoints` is optional. You can override the default endpoints for SNS and SQS, for instance if you are using a local mock.
* `subscriptions` contains the SNS topic names as well as the subscriptions that should be created.

