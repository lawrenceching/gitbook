# Zero Down Time Deployment in Kubernetes

Generally speaking, a deployment consists of 3 steps:  
1. Stop  
2. Deploy  
3. Start  
  
When it comes to zero-down-time deployment, we need to fulfill more details.

**Stop**  
Stop routing new requests  
Consume ongoing requests  
Transfer state for stateful application  
  
**Deploy**  
Deploy new version of application  
  
**Start**  
Warm up \(prepare data, transfer state back, JVM warm-up etc\)  
Start to receive requests

### Zero Down Time Deployment in Kubernetes

In the Kubernetes environment, things are relatively easy. readinessProbe is the major feature we need.  
  
**Stop**  
Your application should handle the exit signal. When app starts to stop, fail the readinessProbe. Therefore k8s knows an instance is not functioning, no more request will be routed to your instance that gonna upgrade.  
  
Existing requests are able to continue until app completes the whole task.  Be aware of the grace period, if request take too long to complete, the instance will be force killed, which will lead to service outage.  
  
**Deploy**  
Update your deployment or run a `helm upgrade`  
  
**Start**  
Set readiness to true when everything is ready.



