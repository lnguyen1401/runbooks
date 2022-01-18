# KubeletDown

## Meaning

This alert signifies issues with the NTP server on a node (mentioned in the alert). When the affected node is a master, this specifically becomes an issue as when the certs are rotated for the apiserver on that node, they fail validation due to the invalid time on that master.


## Impact
This alert is critical. It can lead to the apiserver operator going down and affecting other operators (like monitoring etc) after an extended period of apiserver operator being degraded.

## Diagnosis

1. Start a debug pod on the affected node and check the chronyd service. Start it if it's failing or stopped.

```shell
oc -n default debug node/<affected_node_name>
systemctl status chronyd
systemctl start chonyd
```

if the chronyd service if the service is ready, restart it

```shell
systemctl restart chronyd
```

2. Validate

If successful, starting chronyd should adjust the clock and output something similar to the following:

```shell
Oct 18 19:39:36 ip-100-67-47-86 chronyd[2055318]: System clock wrong by 16422.107473 seconds, adjustment started
Oct 19 00:13:18 ip-100-67-47-86 chronyd[2055318]: System clock was stepped by 16422.107473 seconds
```

Verify the service is running

```shell
systemctl status chronyd
```


## Mitigation
 

