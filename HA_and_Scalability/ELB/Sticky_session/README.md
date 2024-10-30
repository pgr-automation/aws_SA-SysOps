# AWS Elastic Load Balancer (ELB) Sticky Sessions

**Sticky Sessions**, also known as session affinity, is a feature of AWS Elastic Load Balancer (ELB) that ensures a user's requests are consistently routed to the same target within a target group for the duration of their session. This document outlines the use cases, features, and reasons to use sticky sessions.

---

## Use Cases for Sticky Sessions

1. **Web Applications with User Sessions**
   - Applications that maintain user state, such as e-commerce websites, social media platforms, or content management systems, benefit from sticky sessions to ensure users remain on the same instance throughout their session.

2. **Stateful Applications**
   - Applications that require session data to be stored in memory on the server, such as chat applications or online gaming, can use sticky sessions to maintain continuity in user experience.

3. **Short-Lived Sessions**
   - For applications with short-lived sessions, such as booking systems or ticket sales, sticky sessions help ensure that user interactions remain consistent during critical transactions.

4. **Legacy Applications**
   - Legacy applications that were not designed for distributed environments and rely on session data being stored on a single server can utilize sticky sessions to maintain their existing functionality in a load-balanced environment.

---

## Features of Sticky Sessions

1. **Session Affinity**
   - Ensures that all requests from a user are routed to the same target for the duration of their session, providing a seamless experience.

2. **Cookie-Based Implementation**
   - Sticky sessions are typically implemented using cookies. The ELB generates a special cookie (AWSALB) to track sessions and route requests accordingly.

3. **Configurable Timeout**
   - The duration of sticky sessions can be configured, allowing administrators to define how long a user's requests should be routed to the same target after the initial request.

4. **Integration with Auto Scaling**
   - Sticky sessions work seamlessly with Auto Scaling groups, allowing new instances to join or leave the target group without disrupting user sessions.

---

## Why Use Sticky Sessions?

1. **Improved User Experience**
   - By ensuring that users maintain session continuity, sticky sessions enhance the overall user experience, making applications feel more responsive and stable.

2. **Simplified Session Management**
   - Reduces the complexity of session management for applications that require user state to be maintained, as it allows stateful applications to operate within a distributed architecture.

3. **Reduced Latency**
   - By routing requests to the same target, sticky sessions can minimize the latency associated with session state retrieval, as users do not have to re-establish their session state on different instances.

4. **Easier Integration for Legacy Applications**
   - Sticky sessions provide a straightforward way to adapt legacy applications to a load-balanced environment without extensive refactoring or modifications.

5. **Cost-Effectiveness**
   - By improving application performance and user satisfaction, sticky sessions can lead to increased customer retention and conversion rates, contributing to overall business success.

---

## Conclusion

AWS Elastic Load Balancer's sticky sessions feature is a valuable tool for applications requiring session persistence and continuity. By ensuring that user requests are consistently routed to the same target, sticky sessions enhance user experience, simplify session management, and provide seamless integration with Auto Scaling.

For more information, refer to the [AWS ELB Sticky Sessions Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-sticky-sessions.html).


# Create Stickiness

```

1 . Go to Target group 
	- Actions -> Edit attributes -> Stickiness (select LB generated Cookie or application based cookie) 
		- LB generated Cookie -> selectoin stickiness duration 
		- Application based cookie -> selectoin stickiness duration -> app cookie name

```