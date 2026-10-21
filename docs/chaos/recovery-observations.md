# RideFlow — Chaos Engineering: Service Recovery Observations

This document records key observations to evaluate how RideFlow behaves when individual services experience failures. The goal is to understand whether the platform detects faults, limits their impact, and recovers without requiring a complete system restart.

| Experiment | Expected Behavior | Summary |
|---|---|---|
| `ride-service-failure` | Service recovery | When the ride service becomes unavailable, health checks should detect the failure and the recovery mechanism should restore service availability. Dependent requests should fail gracefully instead of causing cascading failures. |
| `pricing-service-latency` | Latency isolation | When the pricing service responds slowly, timeout and circuit-breaker mechanisms should prevent requests from waiting indefinitely and help protect other services from excessive delays. |
| `driver-service-unavailable` | Fault containment | When the driver service becomes unavailable, ride matching may be temporarily affected, but unrelated functionality should continue wherever possible. |
| `api-gateway-restart` | Traffic recovery | After an API gateway restart, health checks and service orchestration should help restore request handling. Clients should receive controlled errors while the gateway is unavailable. |
| `dependency-recovery` | Normal operation restored | Once a failed dependency becomes healthy again, the platform should resume normal request processing without leaving circuit breakers or fault-injection states stuck open. |

## What Should Be Measured?

- **MTTD (Mean Time to Detect):** The average time taken to detect a failure after it occurs.
- **MTTR (Mean Time to Recover):** The average time taken to restore normal service after a failure.
- **Error Rate:** The percentage of requests that fail during the experiment.
- **Latency:** The time taken to process requests, particularly during failure and recovery.
- **Service Availability:** Whether the affected service and its dependent services remain reachable.

## Recovery and Safety Considerations

- Fault injection should be limited to the intended service and test environment.
- Recovery should be verified using service health checks and application metrics.
- Experiments should have a clear abort mechanism to stop faults and restore normal operation.
- Results should be compared against steady-state measurements to identify performance degradation.

## Test Status

The scenarios above describe expected behavior and are not confirmed experiment results. Actual MTTD, MTTR, error rates, and recovery outcomes should be recorded after executing the corresponding experiments through RideFlow's chaos injector.

The purpose of this document is to provide a consistent basis for evaluating fault detection, recovery effectiveness, and the resilience of the overall microservices platform.
