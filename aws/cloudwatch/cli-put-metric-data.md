# aws-cli put-metric-data

Here is an example of how to create CloudWatch metric with one dimension
using the aws cli.

I ran this a few times.

```sh
echo '
[
  {
    "MetricName": "failed_search_count",
    "Value": 1,
    "Unit": "Count",
    "Dimensions": [ {"Name": "stack", "Value": "qa"}]
  }
]' > metric.json

aws cloudwatch put-metric-data \
  --namespace "helix/archive-search" \
  --region us-west-2 \
  --metric-data file:///Users/john.stein1/metric.json
```

Here is how to get the metric data.

```sh
aws cloudwatch get-metric-statistics \
  --namespace "helix/archive-search" \
  --region us-west-2 \
  --metric-name failed_search_count \
  --dimensions Name=stack,Value=qa \
  --start-time 2022-06-03T00:00:00Z \
  --end-time 2022-06-03T17:00:00Z \
  --period 300 \
  --statistics Sum
```

Command output:

```json
{
    "Label": "failed_search_count",
    "Datapoints": [
        {
            "Timestamp": "2022-06-03T16:30:00+00:00",
            "Sum": 2.0,
            "Unit": "Count"
        },
        {
            "Timestamp": "2022-06-03T12:10:00+00:00",
            "Sum": 2.0,
            "Unit": "Count"
        },
        {
            "Timestamp": "2022-06-03T00:25:00+00:00",
            "Sum": 0.0,
            "Unit": "Count"
        }
    ]
}
```
