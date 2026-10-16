# Service Discovery

Spring Cloud Netflix Eureka server for service registration and discovery. The application is configured to listen on port `8761` and not register itself or fetch a registry.

## Architecture and classes

`ServiceDiscoveryApplication` is the Spring Boot entry point and enables the Eureka server with `@EnableEurekaServer`. The Eureka dashboard is normally available at `http://localhost:8761/`.

The inspected configuration shows `userservice` and `productservice` configured as Eureka clients. The payment and email applications do not currently declare Eureka client dependencies/configuration, so they should not be described as registered services.

## Local run

Requires Java 17 and Maven (or the included `mvnw.cmd`):

```powershell
.\mvnw.cmd spring-boot:run
```

Start this service before the configured Eureka clients. Check the dashboard for the instances that actually register.

## Production considerations

Eureka provides a registry and health/instance metadata; it does not itself route every service request. The product service currently calls the auth service using a fixed localhost URL, so service discovery is not used for that call. Replace the fixed URL with a discovery-aware client before multi-host deployment.

Use multiple registry instances for availability, configure authentication/network controls, health checks and leases deliberately, and avoid exposing the dashboard publicly. Document and test service IDs and health status as part of deployment.
