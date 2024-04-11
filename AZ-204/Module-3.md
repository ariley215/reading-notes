# Scale apps in Azure App Service

Learning objectives:

- Identify scenarios for which autoscaling is an appropriate solution.
- Create autoscaling rules for a web app.
- Monitor the effects of autoscaling.

Autoscaling is a cloud system or process that adjusts available resources based on the current demand, adding or removing web servers as needed.

- performs scaling in and out, but does not affect individual instance size (scale up/down)

Autoscaling could be triggered if CPU utilization grows, memory occupancy increases, the number of incoming requests to a service appears to be surging, or some combination of factors.

## Azure App Service Autoscaling

Autoscaling in Azure App Service

- Monitors resource metrics and adjusts number of web servers based on workload
- Triggered by factors like CPU utilization, memory occupancy, and incoming requests
- Defined by user-defined rules
- Can deallocate resources when workload decreases

Benefits of Autoscaling

- Provides elasticity for services
- Improves availability and fault tolerance

Considerations for Autoscaling

- Not effective for resource-intensive processing
- Not the best approach for long-term growth
- Number of instances affects availability even with autoscaling enabled.

Autoscaling Rules:

- Define rules based on thresholds for key metrics like CPU, memory, requests.
- Be careful not to scale up to handle malicious traffic like DDoS attacks(deniol of service) - better to detect and filter those requests.

## Indentify autoscale factors

Autoscaling and the App Service Plan:

- App Service Plans have an instance limit to prevent runaway autoscaling.
- Not all App Service Plan pricing tiers support autoscaling.

Autoscale Conditions:

- Can scale based on a metric (e.g. CPU, memory, disk/HTTP queue length) crossing a threshold.
- Can scale to a specific instance count based on a schedule (time of day, day of week).
- Can combine metric-based and schedule-based autoscaling in the same condition.
- Multiple autoscale conditions can be defined, and any that apply will trigger scaling.

Autoscale Metrics:

- Key metrics include CPU%, Memory%, Disk Queue Length, HTTP Queue Length, Data In/Out.
- Metrics are aggregated over a "time grain" (usually 1 minute) and a configured "duration" (min 5 mins).
- Can use different aggregation methods (avg, max, min, etc.) for the time grain and duration.

Autoscale Actions:

- Scale-out and scale-in actions based on threshold crossings.
- Actions can increment/decrement instances or set to a specific count.
- Autoscale actions have a cool-down period (min 5 mins) to allow the system to stabilize.

Pairing Autoscale Rules:

- Recommend defining paired scale-out and scale-in rules for the same metric.
- Can combine multiple unrelated autoscale rules in a single condition.

## Enabling Autoscale in App Service

- Navigate to App Service plan in Azure portal
- Select **Scale out** in **Settings**
- Select **Custom autoscale**
- Enable autoscaling

**Creating Autoscale Conditions**

- Edit default scale condition or create custom conditions
- Conditions can be metric-based or instance count-based
- Metric-based conditions can specify minimum and maximum instance count
- Custom conditions can include schedules

**Creating Scale Rules**

- Add scale rules to metric-based conditions
- Define criteria for triggering autoscale actions
- Specify autoscale action (scale out or scale in)
- Use metrics, aggregations, operators, and thresholds

**Monitoring Autoscaling Activity**

- Use **Run history** chart in Azure portal
- Track instance count changes over time
- Correlate autoscaling events with resource utilization metrics on **Overview** page

## Explore autoscale best practices

Ensure Adequate Min/Max Instance Limits:

- Make sure the max instance count is different from the min, and provides sufficient headroom.
- If min=max=current instances, no scaling can occur.

Choose Appropriate Metric Statistics:

- Average is the most common statistic to use when scaling based on a metric.

Set Distinct Thresholds for Scale-Out and Scale-In:

- Avoid using the same or similar thresholds for scale-out and scale-in.
- This can cause "flapping" behavior where scaling in and out repeatedly.
- Recommend a larger margin between scale-out and scale-in thresholds.

Multiple Scaling Rules Behavior:

- On scale-out, any rule being met will trigger scaling.
- On scale-in, all rules must be met to trigger scaling.

Set a Safe Default Instance Count:

- The default instance count is used when metrics are unavailable.
- Should be a safe number for the workload.

Configure Notifications:

- Autoscale activity is logged to the Activity Log.
- Can set up alerts on the Activity Log to get notified of scaling events.
- Can also configure email/webhook notifications directly in the autoscale settings.
