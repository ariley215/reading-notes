# Explore API Management

Objectives:

- Describe the components, and their function, of the API Management service.
- Explain how API gateways can help manage calls to your APIs.
- Secure access to APIs by using subscriptions and certificates.
- Create a backend API.

# Azure API Management

## Overview

**API Management** is a comprehensive service in Azure that provides developers with a streamlined way to expose, manage, and monitor their APIs.

Several core functionalities include:

- Developer engagement
- Business insights
- Analytics
- Security and protection

### Key Components

1. **API Gateway**  
   The API Gateway acts as the entry point for API calls and provides the following functionalities:
   - **Routing:** Directs incoming API calls to the appropriate backend services.
   - **Verification:** Validates API keys and other credentials.
   - **Quotas:** Enforces usage quotas and rate limits.
   - **Transformation:** Modifies requests and responses based on defined policies.
   - **Caching:** Stores responses to reduce latency and backend load.
   - **Logging:** Emits logs, metrics, and traces for monitoring.

2. **Management Plane**  
   The Management Plane serves as the administrative interface for:
   - **Provisioning:** Configuring service settings.
   - **Schema Definition:** Defining or importing API schema.
   - **Packaging:** Grouping APIs into products.
   - **Policies:** Setting up quotas and transformations.
   - **Analytics:** Getting insights.
   - **User Management:** Handling user access.

3. **Developer Portal**  
   The Developer Portal is a customizable website for developers to:
   - **Documentation:** Access API documentation.
   - **Interactive Console:** Test APIs.
   - **Account Management:** Create accounts and subscribe for keys.
   - **Analytics:** Monitor usage.
   - **API Definitions:** Download definitions.
   - **Key Management:** Manage API keys.

### Products, Groups, and Developers

**Products:**  

- **Description:** The entities through which APIs are offered to developers.
- **Types:**
  - **Open:** No subscription required.
  - **Protected:** Requires subscription.
- **Subscription Approval:** Either admin-approved or auto-approved.

**Groups:**  

- **Description:** Control the visibility of products.
- **Types:**  
  - **Administrators:** Manage instances, create APIs and products.
  - **Developers:** Authenticated portal users who build apps using APIs.
  - **Guests:** Unauthenticated portal users with read-only access.

**Developers:**  

- **Description:** Represent user accounts in the service.
- **Creation:** Either invited or self-sign-up through the portal.
- **Group Membership:** Each developer belongs to one or more groups.
- **Subscription:** Developers subscribe to products through groups.

### Policies

**Policies**  

- **Description:** Statements executed sequentially on API requests or responses.
- **Popular Policies:**
  - **Format Conversion:** E.g., XML to JSON.
  - **Rate Limiting:** Restrict the number of calls.
- **Policy Expressions:** Used as attribute or text values unless specified otherwise.
  - **Scope:** Can apply globally, to a product, specific API, or operation.

## Key Points to Remember

- **API Management Components:** Understand the three core components.
- **Products and Groups:** Know how products and groups work together.
- **Developers:** Familiarize yourself with how developers interact with products.
- **Policies:** Get comfortable with how policies are defined and executed.

### Study Tips

1. **Understand Core Concepts:** Focus on understanding how the API Gateway, Management Plane, and Developer Portal function together.
2. **Practice with Policies:** Experiment with different policy settings to get a hands-on understanding.
3. **Explore the Developer Portal:** Familiarize yourself with the functionality available to developers.
4. **Review Use Cases:** Consider different scenarios where API Management could be useful.

### **Understanding API Gateways**

---

**Objective:**
Learn how API gateways function within Azure API Management, focusing on managed and self-hosted gateways.

**Key Concepts:**

1. **Role of API Gateways:**
   - API gateways act as a crucial intermediary between clients (users of the API) and backend services.
   - They manage routing of requests from clients to appropriate services, ensuring efficient and secure interactions.
   - Common functionalities include SSL termination, authentication, and rate limiting.

2. **Advantages of Using an API Gateway:**
   - **Simplifies Client Interaction:** Eliminates the need for clients to manage connections to multiple backend services.
   - **Decouples Clients from Services:** Clients do not need to know about the internal structure of backend services, facilitating easier maintenance and updates.
   - **Enhances Security:** Centralizes common security tasks such as authentication and SSL termination, reducing potential attack surfaces.

3. **Managed vs. Self-Hosted Gateways:**
   - **Managed Gateway:**
     - Automatically deployed in Azure for every API Management instance.
     - Suitable for most use cases, especially when backend services are also hosted on Azure.
   - **Self-Hosted Gateway:**
     - A flexible, containerized option that can be deployed off Azure, useful in hybrid and multicloud environments.
     - Allows for managing APIs that are not hosted on Azure, providing the same management capabilities as the managed gateway.

**Usage Scenarios:**

- **Managed Gateway:** Best for applications entirely hosted within Azure, providing streamlined management and minimal configuration.
- **Self-Hosted Gateway:** Ideal for enterprises with significant on-premises or multicloud infrastructure needing unified API management across diverse environments.

### **Understanding API Management Policies**

---

**Objective:**
Gain a comprehensive understanding of how to manipulate API behavior using policies in Azure API Management.

**Key Concepts:**

1. **Role and Function of Policies:**
   - Policies in Azure API Management are collections of statements that modify the behavior of APIs at runtime.
   - These policies can affect both inbound requests and outbound responses as they pass through the gateway.

2. **Policy Definition and Structure:**
   - Policies are defined in XML format and consist of a sequence of directives that apply to different stages of the request/response lifecycle: inbound, backend, outbound, and on-error.
   - An example of a basic policy structure is shown below:

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies>
```

3. **Policy Expressions:**
   - Policy expressions allow dynamic adjustments based on the API's runtime context and can be used within any policy statement.
   - Expressions can be simple C# statements or blocks of C# code that manipulate the request or response. An example using the `set-header` policy is:

     ```xml
     <policies>
         <inbound>
             <base />
             <set-header name="x-request-context-data" exists-action="override">
                 <value>@(context.User.Id)</value>
                 <value>@(context.Deployment.Region)</value>
             </set-header>
         </inbound>
     </policies>
     ```

4. **Applying Policies at Different Scopes:**

   - Policies can be defined globally or at individual API levels. When an API with specific policies is called, both the API-specific and global policies are applied.
   - The order of policy execution can be controlled using the `base` element, as illustrated in the following example:

     ```xml
     <policies>
         <inbound>
             <cross-domain />
             <base />
             <find-and-replace from="xyz" to="abc" />
         </inbound>
     </policies>
     ```

5. **Filtering Response Content:**
   - Policies can also modify the API response based on conditions. The following example filters certain data elements from the response based on the product associated with the request:

     ```xml
     <policies>
       <outbound>
         <base />
         <choose>
           <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals('Starter'))">
             <set-body>
               @{
                 var response = context.Response.Body.As<JObject>();
                 foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
                 response.Property(key).Remove();
                 }
               return response.ToString();
               }
             </set-body>
           </when>
         </choose>
       </outbound>
     </policies>
     ```

### **Creating Advanced Policies in Azure API Management**

---

**Objective:**
Understand the application and configuration of advanced policies in Azure API Management to control API behavior dynamically.

**Key Concepts:**

1. **Control Flow:**
   - Uses the `choose` policy to apply policy statements conditionally based on the outcomes of Boolean expressions, similar to if-then-else or switch constructs in programming.
   - Example of a `choose` policy:

     ```xml
     <choose>
         <when condition="Boolean expression | Boolean constant">
             <!-- one or more policy statements to be applied if the above condition is true -->
         </when>
         <when condition="Boolean expression | Boolean constant">
             <!-- one or more policy statements to be applied if the above condition is true -->
         </when>
         <otherwise>
             <!-- one or more policy statements to be applied if none of the above conditions are true -->
         </otherwise>
     </choose>
     ```

2. **Forward Request:**
   - The `forward-request` policy forwards the incoming request to the backend service as specified in the API settings.
   - Example:

     ```xml
     <forward-request timeout="time in seconds" follow-redirects="true | false"/>
     ```

3. **Limit Concurrency:**
   - Restricts the execution of enclosed policies to no more than the specified number of requests at any time, returning a 429 Too Many Requests status code if exceeded.
   - Example:

     ```xml
     <limit-concurrency key="expression" max-count="number">
         <!-- nested policy statements -->
     </limit-concurrency>
     ```

4. **Log to Event Hub:**
   - Sends messages to an Azure Event Hub for logging purposes. Useful for diagnostics and monitoring.
   - Example:

     ```xml
     <log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
         Expression returning a string to be logged
     </log-to-eventhub>
     ```

5. **Mock Response:**
   - Returns a mocked response directly to the caller, useful for testing and development.
   - Example:

     ```xml
     <mock-response status-code="code" content-type="media type"/>
     ```

6. **Retry:**
   - Retries execution of enclosed policy statements until the condition becomes false or the retry count is exhausted.
   - Example:

     ```xml
     <retry
         condition="boolean expression or literal"
         count="number of retry attempts"
         interval="retry interval in seconds"
         max-interval="maximum retry interval in seconds"
         delta="retry interval delta in seconds"
         first-fast-retry="boolean expression or literal">
             <!-- One or more child policies. No restrictions -->
     </retry>
     ```

7. **Return Response:**
   - Aborts pipeline execution and returns a specified response to the caller.
   - Example:

     ```xml
     <return-response response-variable-name="existing context variable">
       <set-header/>
       <set-body/>
       <set-status/>
     </return-response>
     ```

**Additional Resources:**

- API Management policies provide extensive examples and configurations for effective API governance.
- Consider exploring error handling strategies in API Management policies for robust API setups.

### **Securing APIs Using Subscriptions in Azure API Management**

**Key Concepts:**

1. **Introduction to Subscriptions:**
   - APIs published through Azure API Management are commonly secured using subscription keys. These keys are required when developers make HTTP requests to the APIs.
   - A subscription key is linked to a subscription, which acts as a container for the keys and can be scoped to specific areas.

2. **Subscription Scopes:**
   - **All APIs:** Subscription applies to every API accessible from the gateway.
   - **Single API:** Subscription applies to a single imported API and all its endpoints.
   - **Product:** Subscription applies to a collection of APIs grouped as a product, which can have different access rules and usage quotas.

3. **Using Subscription Keys:**
   - Subscription keys can be passed in HTTP request headers or as query string parameters.
   - The default header for the subscription key is `Ocp-Apim-Subscription-Key`.
   - Example using curl to pass the subscription key in the header:

     ```bash
     curl --header "Ocp-Apim-Subscription-Key: <key string>" https://<apim gateway>.azure-api.net/api/path
     ```

   - Example using curl with the key as a query string:

     ```bash
     curl https://<apim gateway>.azure-api.net/api/path?subscription-key=<key string>
     ```

   - Failure to include a valid subscription key results in a 401 Access Denied response.

4. **Management of Subscription Keys:**
   - Every subscription includes two keys: a primary and a secondary. This dual-key system facilitates seamless key rotation and minimizes downtime.
   - Keys can be regenerated at any time, for instance, if a key is compromised.

5. **Alternative Security Mechanisms:**
   - Besides subscription keys, Azure API Management supports additional security mechanisms such as OAuth 2.0, client certificates, and IP allow listing, providing multiple layers of security.



---
