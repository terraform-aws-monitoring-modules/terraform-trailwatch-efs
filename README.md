# aws-monitoring-efs
This Terraform module creates CloudWatch Log Metric Filters and associated Alarms for monitoring Elastic File Systems (EFS) based on specified event names. It helps ensure that critical changes to EFS are monitored effectively and alerts are sent to a pre-existing SNS topic.

## Features
- Creates CloudWatch Log Metric Filters for specified EFS instances.
- Creates CloudWatch Alarms that trigger based on metrics from the filters.
- Flexible configuration for events to monitor and alarm settings.

## Requirements
- Terraform 1.0 or later
- AWS Provider

## Inputs
| Variable                                     | Description                                                                                          | Type          | Default                                                   |
|----------------------------------------------|------------------------------------------------------------------------------------------------------|---------------|-----------------------------------------------------------|
| `efs_file_system_ids`                       | The list of Elastic File System IDs to monitor.                                                    | `list(string)` | n/a                                                       |
| `efs_file_system_event_names`               | The list of event names to monitor for each Elastic File System.                                   | `list(string)` | `["DeleteFileSystem", "UpdateFileSystem", "CreateMountTarget", "DeleteMountTarget"]` |
| `cw_log_group_name`                         | The name of the CloudWatch log group storing CloudTrail logs.                                      | `string`      | n/a                                                       |
| `cw_metric_filter_namespace`                | The namespace for the CloudWatch metric filter.                                                    | `string`      | `EFS/Monitoring`                                         |
| `cw_metric_filter_value`                    | The value to publish to the CloudWatch metric.                                                     | `string`      | `1`                                                       |
| `cw_metric_filter_alarm_comparison_operator` | The comparison operator for the CloudWatch metric filter alarm.                                     | `string`      | `GreaterThanOrEqualToThreshold`                          |
| `cw_metric_filter_alarm_evaluation_periods` | The number of periods over which data is compared to the specified threshold.                       | `number`      | `1`                                                       |
| `cw_metric_filter_alarm_period`             | The period in seconds over which the specified statistic is applied.                                | `number`      | `300`                                                     |
| `cw_metric_filter_alarm_statistic`          | The statistic to apply to the alarm's associated metric.                                           | `string`      | `Sum`                                                    |
| `cw_metric_filter_alarm_threshold`          | The value against which the specified statistic is compared.                                       | `number`      | `1`                                                       |
| `cw_metric_filter_alarm_actions`            | The list of actions to execute when the alarm transitions into an ALARM state.                      | `list(string)` | `[]`                                                      |

## Simple Example
```hcl
module "aws_monitoring_efs" {
  source                         = "path/to/module"
  efs_file_system_ids            = ["fs-12345678", "fs-87654321"]
  cw_log_group_name              = "the-cloudtrail-log-group"
  cw_metric_filter_alarm_actions = ["arn:aws:sns:region:account-id:sns-topic"]
}
```

## Advanced Example
```hcl
module "aws_monitoring_efs" {
  source                                     = "path/to/module"
  efs_file_system_ids                        = ["fs-12345678", "fs-87654321"]
  efs_file_system_event_names                = ["DeleteFileSystem", "UpdateFileSystem"]
  cw_log_group_name                          = "the-cloudtrail-log-group"
  cw_metric_filter_namespace                 = "EFS/Monitoring"
  cw_metric_filter_value                     = "1"
  cw_metric_filter_alarm_comparison_operator = "GreaterThanOrEqualToThreshold"
  cw_metric_filter_alarm_evaluation_periods  = 1
  cw_metric_filter_alarm_period              = 300
  cw_metric_filter_alarm_statistic           = "Sum"
  cw_metric_filter_alarm_threshold           = 1
  cw_metric_filter_alarm_actions             = ["arn:aws:sns:region:account-id:sns-topic"]
}
```

## Changelog
For a detailed list of changes, please refer to the [CHANGELOG.md](CHANGELOG.md).

## License
This module is licensed under the [MIT License](LICENSE).
