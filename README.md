# Cloud Native 12 Factors

# Factor 1: SCM and revision control
- The code base itself must be tracked in a version control system
- Single application on single repo is the key
- Often deploys, easy to be able to trace bugs caused by which deployment

# Factor 2: Manage dependencies
- Explicitly declare and isolate dependencies
- Each app should run in its own sandbox, don't rely on anything from its runtime container
- App should run anywhere, in any container and on any operating system

# Factor 3: Application Configuration
- Store config in the environment variables
- Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict separation of config from code
- Configuration includes all values needed by your app, but specific to the environment
- Externalize configs from the applications: the config values are not embedded in the code itself
- The application itself utilizes a placeholder in the code for the config value

# Factor 4: Backing Services
- Treat backing services as attached resources
- Backing service is any service that the app consumes over the network as part of its normal operation
- In 12-factors app, remote services are treated the same as the local services
- A deploy of the 12-factor app should be able to swap out a local MySQL database with one managed by a third party (such as Amazon RDS) without any changes to the app’s code
- Each distinct backing service is a resource. For example, a MySQL database is a attached resource
- Resources can be attached to and detached from deploys at will

# Factor 5: Build, Release and Run (CI-CD)
- Strictly separate build and run stages
- Build = convert code to an executable + fetches vendors dependencies
- Release = build + configs
- Run = run app in the execution environment by launching some set of the selected apps

# Factor 6: Run Processes
- Execute the app as one or more stateless processes
- The app is executed in the execution environment as one or more processes
- Applications themselves are deployed as one or more processes instead of embedding them in another process (Tomcat server)
- Twelve-factor processes are stateless and share-nothing. Any data that needs to persist must be stored in a stateful backing service, typically a database
- Sticky sessions are a violation of twelve-factor and should never be used or relied upon

# Factor 7: Port Binding
- Export services via port binding
- The twelve-factor app is completely self-contained, The web app exports HTTP as a service by binding to a port, and listening to requests coming in on that port
- Note also that the port-binding approach means that one app can become the backing service for another app, by providing the URL to the backing app as a resource handle in the config for the consuming app

# Factor 8: Concurrency
- Scale out via the process model
- The share-nothing, horizontally partitionable nature of twelve-factor app processes means that adding more concurrency is a simple and reliable operation
- For example, HTTP requests may be handled by a web process, and long-running background tasks handled by a worker process

# Factor 9: Disposability
- Maximize robustness with fast startup and graceful shutdown
- The twelve-factor app’s processes are disposable, meaning they can be started or stopped at a moment’s notice
- Processes should strive to minimize startup time: provides more agility for the release process and scaling up; and it aids robustness
- Processes shut down gracefully when they receive a SIGTERM signal from the process manager

# Factor 10: Dev/prod parity
--> Keep development, staging, and production as similar as possible

The time gap: A developer may work on code that takes days, weeks, or even months to go into production.
The personnel gap: Developers write code, ops engineers deploy it.
The tools gap: Developers may be using a stack like Nginx, SQLite, and OS X, while the production deploy uses Apache, MySQL, and Linux.

- The twelve-factor app is designed for continuous deployment by keeping the gap between development and production small
- Make the time gap small: a developer may write code and have it deployed hours or even just minutes later
- Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production
- Make the tools gap small: keep development and production as similar as possible

# Factor 11: Logs
--> Treat logs as event streams
- A twelve-factor app never concerns itself with routing or storage of its output stream
- Logs are the stream of aggregated, time-ordered events collected from the output streams of all running processes and backing services

# Factor 12: Admin processes
--> Run admin/management tasks as one-off processes
- One-off admin processes should be run in an identical environment as the regular long-running processes of the app
- Twelve-factor strongly favors languages which provide a REPL shell out of the box, and which make it easy to run one-off scripts
