# Twelve-Factor App

The [Twelve-Factor App](https://12factor.net/) describes practices for building portable, maintainable services. In brief:

1. Track each application in version control and deploy it to multiple environments.
2. Declare dependencies explicitly and isolate them from the host system.
3. Keep environment-specific configuration outside the codebase.
4. Access databases, queues, and other backing services through configuration.
5. Separate building artifacts, creating releases, and running processes.
6. Avoid relying on local process state between requests.
7. Let the application expose its service on a configured port.
8. Increase capacity by running more processes.
9. Support quick startup and graceful shutdown.
10. Reduce differences between development and production.
11. Emit logs as streams for the execution environment to collect.
12. Run maintenance tasks using the same release and environment as the application.
