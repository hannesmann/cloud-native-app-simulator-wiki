# Input File

HydraGen uses a standard taxonomy with hierarchical structure, which is the input to the Generator module. The input to this module must be given in JSON format following the taxonomy described here.


## Taxonomy for overall application description

```json
{
  "services": [
    {
      "name": "<string>",
      "clusters": [
        {
          "cluster": "<string>",
          "replicas": <integer>,
          "namespace": "<string>",
          "node": "<string>",
          "annotations": [...]
        }
      ],
      "resources": {
        "limits": {
          "memory": "<string:bytes>",
          "cpu": "<string:cores>"
        },
        "requests": {
          "memory": "<string:bytes>",
          "cpu": "<string:cores>"
        }
      },
      "processes": <integer>,
      "threads": <integer>,
       "readiness_probe": <integer>,
      "endpoints": [...]
    }
  ],
  ...
}
```

## Taxonomy for endpoint description

```json
"endpoints": [
  {
    "name": "<string>",
    "protocol": "<string:http|grpc>",
    "execution_mode": "<string:sequential|parallel>",
    "cpu_complexity": {
      "execution_time": <float:seconds>
    },
    "network_complexity": {
      "forward_requests": "string:synchronous|asynchronous",
      "response_payload_size": <integer:chars>,
      "called_services": [
        {
          "service": "<string>",
          "port": "<string>",
          "endpoint": "<string>",
          "protocol": "<string:http|grpc>",
          "traffic_forward_ratio": <integer>,
          "request_payload_size": <integer:chars>
        }
      ]
    }
  }
]
```

# Examples

Examples for simple and complex applications generated with HydraGen can be found [here](generator/examples).