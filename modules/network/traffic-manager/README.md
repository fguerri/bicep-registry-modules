# Azure Traffic Manager

Bicep module for creating a Azure Traffic Manager Profile with endpoints and monitor configurations.

## Description

Azure Traffic Manager is a traffic management solution that enables the distribution of incoming traffic across multiple endpoints.
This can help to improve the performance and availability of applications by directing traffic to the most optimal endpoint based on routing methods such as geographic proximity or performance.
Azure Traffic Manager can be used to distribute traffic between Azure regions, global data centers, or on-premises data centers.

This Bicep module provides the ability to create a new Azure Traffic Manager Profile or use an existing one.
The module allows for the addition of endpoints to the Traffic Manager Profile, and the configuration of monitoring settings for the endpoints.
The endpoints can be specified as an array of objects, each representing an endpoint with a name, target URL, endpoint status, and endpoint location.
The module also allows for the configuration of monitor settings such as protocol, port, path, and expected status code ranges.

## Parameters

| Name                    | Type     | Required | Description                                                                                                                                                       |
| :---------------------- | :------: | :------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `prefix`                | `string` | No       | Prefix of Traffic Manager Profile Name                                                                                                                            |
| `name`                  | `string` | No       | Name of Traffic Manager Profile Resource                                                                                                                          |
| `trafficManagerDnsName` | `string` | No       | Relative DNS name for the traffic manager profile, must be globally unique.                                                                                       |
| `endpoints`             | `array`  | No       | An array of objects that represent the endpoints in the Traffic Manager profile. {name: string, target: string, endpointStatus: string, endpointLocation: string} |
| `monitorConfig`         | `object` | No       | An object that represents the monitoring configuration for the Traffic Manager profile.                                                                           |

## Outputs

| Name | Type   | Description                           |
| :--- | :----: | :------------------------------------ |
| id   | string | Traffic Manager Profile Resource ID   |
| name | string | Traffic Manager Profile Resource Name |

## Examples

### Example 1

This example creates a new Traffic Manager profile with the name "my-traffic-manager" and the DNS name "my-traffic-manager-dns".

```bicep
param name string = 'my-traffic-manager'
param dnsName string = 'my-traffic-manager-dns'

module trafficManager 'br/public:network/traffic-manager:1.0.1' = { = {
  name: 'myTrafficManager'
  params: {
    name: name
    trafficManagerDnsName: dnsName
  }
}
```

### Example 2

In this example, we're creating a new Azure Traffic Manager Profile named "my-traffic-manager-profile" with a globally unique DNS name "my-traffic-manager-dns-name". We're also creating two endpoints with the names "my-endpoint-1" and "my-endpoint-2", each with a target URL and location. We're using the traffic-manager.bicep module to create the Traffic Manager Profile and endpoints. Once the module has been executed, the name output will contain the name of the created Traffic Manager Profile.

```bicep
param name string = 'my-traffic-manager-profile'
param trafficManagerDnsName string = 'my-traffic-manager-dns-name'
param endpoints array = [
  {
    name: 'my-endpoint-1',
    target: 'http://myendpoint1.example.com',
    endpointStatus: 'Enabled',
    endpointLocation: 'eastus'
  },
  {
    name: 'my-endpoint-2',
    target: 'http://myendpoint2.example.com',
    endpointStatus: 'Enabled',
    endpointLocation: 'westus'
  }
]

module trafficManagerProfile 'br/public:network/traffic-manager:1.0.1' = { = {
  name: 'trafficManagerProfile'
  params: {
    name: name
    trafficManagerDnsName: trafficManagerDnsName
    endpoints: endpoints
  }
}
```