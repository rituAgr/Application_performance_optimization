### How requests are handled in springboot application?
   Each incoming request requires a thread for the duration of that request. If more simultaneous 
requests are received than can be handled by the currently available request processing threads, 
additional threads will be created up to the configured maximum (the value of 
the maxThreads attribute). If still more simultaneous requests are received, they are stacked up
inside the server socket created by the Connector, up to the configured maximum (the value of 
the acceptCount attribute). Any further simultaneous requests will receive "connection refused
(ERR_CONNECTION_REFUSED)" errors, until resources are available to process them. 
   * [Link for Hikari connection details](https://github.com/brettwooldridge/HikariCP)
   * The number of request that can be handled by a springboot instance server is configured as -
       ```server.tomcat.threads.max=xxx```
   * [Link to tomcat document](https://tomcat.apache.org/tomcat-8.5-doc/config/http.html)
   * [Link to youtube for thread explanation](https://www.youtube.com/watch?v=76MEPTM2ARI&list=PLqq-6Pq4lTTbXZY_elyGv7IkKrfkSrX5e&index=7&ab_channel=JavaBrains)

### Ways to improve performance on backend

1. Increase the thread pool size via  ```server.tomcat.threads.max=xxx``` config. However,
   this is temporary fix. As the risinf request call will pick up all the threads from
   the increase size pool. Further, users click Refresh page, sending duplicate requests
   It further aggravates the problem
2. For outgoing api calls from application, we can configure timeout at restTemplate level.
  This will avoid threads being blocked for very long time and hence become free quickly for 
 long call to serve other calls.
 [youtube explaination Link](https://www.youtube.com/watch?v=6osL1aAIXU4&list=PLqq-6Pq4lTTbXZY_elyGv7IkKrfkSrX5e&index=8&ab_channel=JavaBrains)
3. If the database is used (lets say mysql), then the connection pool size & timeout can be 
  configured at application level for databases connections.
   [Link for Hikari connection details](https://github.com/brettwooldridge/HikariCP)
   Further, the table in database can be indexed based on access pattern. This will 
   further enhance the performance for database interactions.
4. Cache the static data on our side and implement cron job for late night to run and update

### Ways to improve performance on frontend
1. Make the api call for expandable section only when it is expanded 
