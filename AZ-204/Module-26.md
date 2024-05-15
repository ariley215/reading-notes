# Monitor App Performance

Objectives:

- Explain how Azure Monitor operates as the center of monitoring in Azure.
- Describe how Application Insights works and how it collects events and metrics.
- Instrument an app for monitoring, perform availability tests, and use Application Map to help you monitor performance and troubleshoot issues.

## Explore Application Insights

### Key Features

| Feature                      | Description                                                                                     |
|------------------------------|-------------------------------------------------------------------------------------------------|
| **Live Metrics**             | Observe real-time activity from your deployed application without impacting the host environment. |
| **Availability**             | Also known as “Synthetic Transaction Monitoring”, it tests the availability and responsiveness of application endpoints over time. |
| **GitHub or Azure DevOps Integration** | Create work items in GitHub or Azure DevOps based on Application Insights data.              |
| **Usage**                    | Understand user interaction and identify popular features within your application.             |
| **Smart Detection**          | Automatically detect failures and anomalies through proactive telemetry analysis.             |
| **Application Map**          | Provides a top-down view of application architecture, highlighting component health and responsiveness. |
| **Distributed Tracing**      | Visualize the end-to-end flow of executions or transactions within your application.           |

### What Application Insights Monitors

Application Insights collects a wide range of metrics and telemetry data to provide a comprehensive view of application activities and health:

- **Request Rates, Response Times, and Failure Rates**: Identify popular pages, peak usage times, and performance issues.
- **Dependency Rates, Response Times, and Failure Rates**: Monitor the impact of external services on application performance.
- **Exceptions**: Analyze aggregated statistics or drill into specific instances to inspect stack traces and related requests.
- **Page Views and Load Performance**: Gathered from user browsers.
- **AJAX Calls from Web Pages**: Track rates, response times, and failure rates.
- **User and Session Counts**: Measure user engagement.
- **Performance Counters**: Monitor CPU, memory, and network usage on Windows or Linux servers.
- **Host Diagnostics**: Collect diagnostics from Docker or Azure environments.
- **Diagnostic Trace Logs**: Correlate trace events with requests for in-depth analysis.
- **Custom Events and Metrics**: Track business-specific events such as items sold or games won.

### Getting Started with Application Insights

Application Insights is hosted within Microsoft Azure, and telemetry data is sent there for analysis and presentation.

It's free to sign up, and the basic pricing plan incurs no charges until your application usage grows significantly.

#### Ways to Get Started

- **At Runtime**: Instrument your web app on the server without updating the code. Ideal for already deployed applications.
- **At Development Time**: Add Application Insights to your code to customize telemetry collection.
- **Instrument Web Pages**: Collect page views, AJAX calls, and other client-side telemetry.
- **Mobile App Integration**: Use Visual Studio App Center to analyze mobile app usage.
- **Availability Tests**: Regularly ping your website from Azure servers to monitor availability.

## Discover Log-Based Metrics

Application Insights log-based metrics allow you to analyze the health of your monitored applications, create powerful dashboards, and configure alerts. These metrics come in two forms:

- **Log-Based Metrics:** Translated into Kusto queries from stored events.
- **Standard Metrics:** Stored as pre-aggregated time series.

### Log-Based Metrics

Log-based metrics provide deep analytical and diagnostic capabilities:

- **Manual and Automatic Collection:** Developers can manually send events via the SDK or rely on auto-instrumentation.
- **Comprehensive Event Storage:** All collected events are stored as logs, allowing detailed analysis and diagnostics.
- **Analytical Value:** Logs provide exact counts and detailed traces, improving visibility into application health and usage. This includes counting requests to specific URLs and tracing exceptions and dependency calls.

### Pre-Aggregated Metrics

Pre-aggregated metrics are stored as time series with key dimensions, offering better performance:

- **Efficient Storage:** Metrics are stored as pre-aggregated time series, improving query performance and reducing compute power requirements.
- **Real-Time Capabilities:** Enables near real-time alerting and responsive dashboards.
- **SDK Support:** Newer SDKs (Application Insights 2.7 SDK or later for .NET) pre-aggregate metrics during collection, ensuring accuracy is unaffected by sampling or filtering.

### Coexistence of Metrics

Both log-based and pre-aggregated metrics coexist in Application Insights:

- **Terminology:** Pre-aggregated metrics are labeled as "Standard metrics (preview)" and traditional metrics as "Log-based metrics".
- **SDK Implementation:** Newer SDKs pre-aggregate metrics, while the backend aggregates events for older SDKs.
- **Accuracy:** Ingestion sampling does not affect the accuracy of pre-aggregated metrics.
- **Cost Efficiency:** Pre-aggregated metrics result in less data ingestion, reducing costs.

## Instrument an App for Monitoring

Application Insights can be enabled through either Auto-Instrumentation (agent) or by adding the Application Insights SDK to your application code.

### Auto-Instrumentation

Auto-instrumentation is the preferred method for enabling Application Insights. 

- It requires no developer investment and eliminates the overhead of updating the SDK. - This method is especially useful when you don't have access to the application's source code.

To enable auto-instrumentation:

- Simply enable and configure the agent as needed.
- The agent automatically collects telemetry data.

For an updated list of services supported by auto-instrumentation, visit the official documentation.

### Enabling via Application Insights SDKs

You need to install the Application Insights SDK in the following circumstances:

- Custom events and metrics are required.
- You need control over the flow of telemetry.
- Auto-Instrumentation isn't available due to language or platform limitations.

Steps to use the SDK:

1. Install the Application Insights SDK package in your application.
2. Instrument the web app, background components, and JavaScript within the web pages.
3. Ensure the application and its components send telemetry data to an Application Insights resource using a unique token.

The SDK supports distributed tracing natively for .NET, .NET Core, Java, Node.js, and JavaScript. Any technology can also be tracked manually using `TrackDependency` on the `TelemetryClient`.

### Enable via OpenCensus

Application Insights supports distributed tracing through OpenCensus

- An open source, vendor-agnostic library for metrics collection and distributed tracing.
- OpenCensus allows for seamless integration with popular technologies such as Redis, Memcached, and MongoDB.

Benefits of using OpenCensus with Application Insights:

- **Vendor-Agnostic:** Works with multiple vendors and technologies.
- **Metrics Collection:** Collects detailed metrics for thorough analysis.
- **Distributed Tracing:** Enables tracking of operations across services, providing a complete view of application performance.

## Select an Availability Test

After deploying your web app or website, set up recurring tests to monitor availability and responsiveness.

- Application Insights sends web requests to your application at regular intervals from various global points and alerts you if the application isn't responding or responds too slowly.

### Availability Test Types

You can set up availability tests for any HTTP or HTTPS endpoint accessible from the public internet.

- No changes are required to the website you're testing.
- It doesn't have to be a site that you own.
- You can even test the availability of a REST API your service depends on.
- You can create up to 100 availability tests per Application Insights resource.

#### URL Ping Test (Classic)

- **Purpose:**
  - Validate whether an endpoint is responding.
  - Measure associated performance.
- **Features:**
  - Custom success criteria.
  - Parsing dependent requests.
  - Allowing retries.
- **Setup:**
  - Create this test through the Azure portal.

#### Standard Test (Preview)

- **Purpose:**
  - Single request test similar to the URL ping test.
- **Features:**
  - SSL certificate validity and proactive lifetime check.
  - HTTP request verb (e.g., GET, HEAD, POST).
  - Custom headers.
  - Custom data associated with your HTTP request.

#### Custom TrackAvailability Test

- **Purpose:**
  - Custom application to run availability tests using the `TrackAvailability()` method.
- **Setup:**
  - Send the results to Application Insights for more flexible and complex test scenarios.

> **Note:** Multi-step tests are available through Visual Studio 2019. The Custom TrackAvailability test is the long-term supported solution for multi-request or authentication test scenarios.

### DNS Considerations

The URL ping test relies on the public internet DNS infrastructure to resolve domain names of tested endpoints.

- If using private DNS, ensure that public domain name servers can resolve every domain name of your test.
- When this isn't possible, use custom TrackAvailability tests instead.

## Troubleshoot App Performance by Using Application Map

Application Map helps you identify performance bottlenecks or failure hotspots across all components of your distributed application.

### Key Features

- **Node Representation:**
  - Each node represents an application component or its dependencies.
  - Displays health KPIs and alerts status.
  - Click through to detailed diagnostics, such as Application Insights events.
  - Access Azure diagnostics for Azure services (e.g., SQL Database Advisor recommendations).

- **Component Definition:**
  - Independently deployable parts of a distributed/microservices application.
  - Developers and operations teams have code-level visibility or access to telemetry.
  - Different from "observed" external dependencies like SQL, Event Hubs, etc.
  - Can run on multiple server/role/container instances.
  - Can be separate Application Insights instrumentation keys or different roles reporting to a single key.

- **Full Application Topology:**
  - Displays across multiple levels of related application components.
  - Finds components by following HTTP dependency calls between servers with the Application Insights SDK installed.

### Component Discovery

- **Progressive Discovery:**
  - On initial load, a set of queries discovers related components.
  - Button at the top-left updates with the number of discovered components.
  - Clicking "Update map components" refreshes the map with all discovered components.

- **Initial Load:**
  - If all components are roles within a single Application Insights resource, discovery isn't required.
  - The initial load includes all components.

![Application Map Initial Load](https://learn.microsoft.com/en-us/training/modules/monitor-app-performance/media/application-map.png)

### Visualizing Complex Topologies

- **Visualization:**
  - Visualize complex topologies with hundreds of components.
  - Click any component to see related insights and access performance and failure triage for that component.

![Component Details in Application Map](https://learn.microsoft.com/en-us/training/modules/monitor-app-performance/media/application-map-component.png)

### Customization

- **Cloud Role Name Property:**
  - Application Map uses the cloud role name property to identify components.
  - Manually set or override the cloud role name to change what gets displayed on the Application Map.
