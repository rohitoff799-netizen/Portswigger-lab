# Lab: Blind OS command injection with time delays

## Description
This lab contains a blind OS command injection vulnerability in the feedback function. The application executes a shell command containing user-supplied details, but does not return any output.

## Analysis
Since we don't get any output, we need to use a technique that provides a visible side effect. Time delays are perfect for this. We can use the `sleep` command to make the server wait for a specific amount of time. If the response is delayed by that amount, we know the command was executed.

## Exploitation
I need to cause a 10-second delay.

### Steps
1. Navigate to the "Submit feedback" page.
2. Intercept the submission request.
3. The parameters are `name`, `email`, `subject`, and `message`.
4. Inject the `sleep` command into the `email` parameter using the `||` separator:
   `email=test||sleep 10||`
5. Send the request and observe that the response takes 10 seconds to arrive.

## Payload
```text
email=test||sleep 10||
```

## Takeaways
- Blind vulnerabilities require inferring the result of an attack based on the application's behavior (e.g., time taken to respond, changes in the UI).
- Time-based detection is a reliable way to confirm blind vulnerabilities.
