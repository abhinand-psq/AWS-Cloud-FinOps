# AWS EC2 Auto Scaling & Cloud Resource Optimization

A hands-on AWS project demonstrating **dynamic EC2 capacity management** using an Auto Scaling Group and Amazon CloudWatch. The group adjusts compute capacity in response to CPU utilization, helping align provisioned resources with workload demand.

> **Validation completed:** The target tracking policy automatically scaled the group from **1 to 2 EC2 instances** when CPU utilization rose above its 50% target. After CPU utilization returned to a low level, the policy scaled the group back from **2 to 1 instance**. Both instances reached **3/3 status checks passed** during the scale-out test, and the remaining instance was healthy after scale-in.

## Project goals

- Configure EC2 Auto Scaling to respond to workload changes without manually changing desired capacity.
- Use CloudWatch CPU metrics with a target tracking scaling policy.
- Validate automatic scale-out and scale-in behavior.
- Capture AWS console and metric evidence for a reproducible project demonstration.

## AWS services

- **Amazon EC2** — virtual machine instances
- **EC2 Auto Scaling** — automatic capacity adjustment
- **Amazon CloudWatch** — CPU utilization metrics and scaling signals
- **AWS IAM** — permissions used by the AWS configuration

## Architecture and scaling behavior

The EC2 instances are managed by an Auto Scaling Group. A CloudWatch-backed **Target Tracking Scaling Policy** maintains average CPU utilization around **50%**.

- When average CPU utilization rises above the target, the policy can increase desired capacity.
- When demand falls and utilization remains below the target, the policy can reduce capacity, subject to group settings.
- The policy has **scale-in enabled** and an instance warm-up period of **300 seconds**.
- The group is configured with capacity limits to control the number of instances.

## Implementation evidence

Screenshots are stored in `./screenshots/`. The image filenames below are the expected README references; make sure your local files use these names.

### 1. Initial provisioning

The Activity History record documents the initial capacity change from 0 to 1 after a user request. This is initial provisioning evidence, **not** the automatic scale-out event.

![Initial Auto Scaling provisioning](./screenshots/autoscaling-initial-provisioning.png)

### 2. Target tracking policy

The policy is enabled and targets average CPU utilization of 50%. Scale-in is enabled, with 300 seconds of instance warm-up.

![Target tracking policy configuration](./screenshots/autoscaling-target-tracking-policy.png)

### 3. Before scale-out — one instance

The initial EC2 state shows one running instance before the CPU load test.

![EC2 instance before scale-out](./screenshots/autoscaling-before-scale-out.png)

### 4. CPU utilization rises above target

During the load test, CloudWatch CPU utilization reached approximately **80.97%**, exceeding the configured 50% target.

![CPU utilization during scale-out test](./screenshots/autoscaling-cpu-scale-out.png)

### 5. Automatic scale-out — Activity History

The Activity History shows the target tracking policy changing desired capacity from **1 to 2** and launching another EC2 instance. This is evidence of policy-triggered scaling, rather than a manual capacity change.

![Target tracking scale-out activity](./screenshots/autoscaling-scale-out-activity.png)

### 6. After scale-out — two healthy instances

The Auto Scaling Group reached two running EC2 instances. Both instances show **3/3 status checks passed**.

![Two healthy EC2 instances after scale-out](./screenshots/autoscaling-after-scale-out.png)

### 7. Automatic scale-in — Activity History

After CPU utilization returned below the target, the CloudWatch `AlarmLow` alarm triggered the target tracking policy. The Activity History records the group reducing desired capacity from **2 to 1** and terminating an instance.

![Target tracking scale-in activity](./screenshots/autoscaling-scale-in-activity.png)

### 8. After scale-in — one healthy instance

The EC2 instance list confirms that the group returned to one running `t3.micro` instance, with **3/3 status checks passed**.

![One healthy EC2 instance after scale-in](./screenshots/autoscaling-after-scale-in.png)

### 9. CPU utilization after scale-in

The CloudWatch CPU utilization graph shows the workload spike followed by utilization returning to a low baseline after the load test.

![CPU utilization after scale-in](./screenshots/autoscaling-cpu-after-scale-in.png)

## Measurable outcomes

| Measure | Observed result |
|---|---|
| CPU target tracking threshold | 50% average CPU utilization |
| CPU during scale-out test | Approximately 80.97% |
| Capacity before scale-out | 1 EC2 instance |
| Capacity after scale-out | 2 EC2 instances |
| Capacity after scale-in | 1 EC2 instance |
| Instance health after scale-out | Both instances passed 3/3 status checks |
| Instance health after scale-in | Remaining instance passed 3/3 status checks |
| Instance warm-up configured | 300 seconds |
| Scaling workflow validated | Automatic scale-out and scale-in |

The project demonstrates elastic capacity management that can help reduce unnecessary compute capacity during lower demand. **No specific cost savings percentage is claimed**, because actual cost reduction was not measured.

## Notes

- The scaling events were triggered by the target tracking policy; capacity was not manually changed during the evidence capture.
- The initial user-requested provisioning event is distinct from the policy-triggered scale-out activity.
- Scaling behavior depends on CloudWatch metric evaluation, instance warm-up, cooldown behavior, and the Auto Scaling Group's minimum and maximum capacity.
