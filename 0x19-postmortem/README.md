# Postmortem: Service Outage Due to Database Connection Pool Exhaustion

### A post-mortem is typically associated with a project failure or serious incident, such as an IT service outage resulting in downtime or other business consequences.

### Issue Summary
- **Outage Duration**: August 10, 2024, 2:30 PM - 3:15 PM GMT (45 minutes)
- **Impact**: The main e-commerce platform was down. Users were unable to browse products, make purchases, or log in. Approximately 85% of users were affected during this period, resulting in the loss of potential transactions and poor customer experience.
- **Root Cause**: Exhaustion of the database connection pool due to a sudden spike in traffic without adequate scaling of the connection pool or handling of idle connections.

---

### Timeline
- **2:30 PM GMT** - Monitoring system triggered an alert for high error rates and failed API requests across the e-commerce platform.
- **2:35 PM GMT** - On-call engineer identified 503 errors on key APIs (product listings and user authentication) and initiated further investigation.
- **2:40 PM GMT** - Backend service logs showed numerous database connection timeout errors. The engineering team suspected a possible issue with the database server.
- **2:45 PM GMT** - The database team checked the health of the database but found it fully operational, leading the investigation to focus on the application layer.
- **2:50 PM GMT** - Investigation led to the discovery that the connection pool on the application was saturated, leading to a backlog of requests.
- **3:00 PM GMT** - Connection pool size was increased, and idle connections were flushed.
- **3:15 PM GMT** - Platform services were fully restored and monitored for further issues.

---

### Root Cause and Resolution

#### Root Cause
The database connection pool was configured with a fixed number of connections, which was insufficient to handle a sudden and unexpected spike in traffic. As user requests surged, the application began to exhaust the available connections. Due to improper handling of idle connections, the pool became saturated, leading to a backlog and eventual timeout errors. These cascading failures resulted in service unavailability for most users.

#### Resolution
To resolve the issue, the engineering team:
- Increased the size of the database connection pool to allow more concurrent connections.
- Cleared idle or stale connections that were hogging the pool, ensuring the application could resume processing new requests.
- Implemented connection retries and timeouts at the application level to prevent long-waiting requests from occupying the pool.

---

### Corrective and Preventative Measures

#### Improvements/Fixes
- **Dynamic Scaling**: Implement auto-scaling for database connections based on traffic patterns to avoid saturation under high load.
- **Idle Connection Management**: Implement better management of idle connections, ensuring they are closed or reused more effectively.
- **Load Testing**: Improve load testing processes to simulate sudden traffic spikes and ensure connection pool limits are adequate for peak conditions.
  
#### Tasks
1. **Increase database connection pool size** in the production environment to handle larger traffic volumes.
2. **Add auto-scaling policies** for the application’s database connection pool to dynamically adjust based on traffic load.
3. **Patch application** to handle connection retries and timeouts to prevent long-lived idle connections.
4. **Update monitoring tools** to trigger earlier alerts when connection pool usage exceeds 80% capacity.
5. **Run load tests** on the platform simulating peak traffic, verifying that connection pool scaling and retries work as expected.
6. **Review database usage patterns** and optimize slow queries to reduce connection duration.

By taking these corrective actions, the engineering team aims to ensure that similar outages do not occur in the future and the platform is better equipped to handle traffic spikes.
