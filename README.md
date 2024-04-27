# mule-circuit-breaker-implementation
Circuit breaker implementation using Groovy Script in Mule 4

## About Project
Demonstrates working principles of a simple circuit breaker. If any conection error from downstream API, invokes Groovy script to shuts down the flow.
Exposes an endpoint to toggle the flow state, which can be used to start the flow at a later point in time either by manual intervention or through automated API.

## Setting Up Prerequisites
Download and Install Grrovy Module from Exchange into Anypoint Studio.

## Setting Up Mulesoft Application
- From Anypoint Studio create a new Mule Project
- Create a flow named 'circuitbreakerFlow' which invokes an external endpoint. In this HTTP Requestor calls an unavailable endpoint inorder to demonstrate circuit breaker.
  On HTTP:CONNECTIVITY error it invokes error handler which in turn executes below Grrovy script asynchronously.
  **Groovy Script:**
  ```
    flow = registry.lookupByName("circuitbreakerFlow").get();
    if (flow.isStarted())
  	  flow.stop()
   ```
 - Create another flow with listener which executes below Grrovy script to toggle back the state.
  **Groovy Script:**
  ```
    flow = registry.lookupByName("circuitbreakerFlow").get();
    if (flow.isStarted())
  	  flow.stop()
    else
      flow.start()
   ```
## Working
#### Main Endpoint
http://localhost:8081/mainflow 
#### Response 
  ```
  {
    "message": "Error response returned from External System; Proceeding to Stop the main flow using circuit breaker"
  }
  ```

#### Restart Flow Endpoint
http://localhost:8081/toggle 

#### Response
  ```
  {
    "message": "Successfully enabled circuitbreakerFlow"
  }
  ```
