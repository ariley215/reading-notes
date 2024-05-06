# Explore API Management

Objectives:

- Describe the components, and their function, of the API Management service.
- Explain how API gateways can help manage calls to your APIs.
- Secure access to APIs by using subscriptions and certificates.
- Create a backend API.

# Azure API Management - Study Guide

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


