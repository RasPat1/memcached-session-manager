A [tomcat](http://tomcat.apache.org/) high-availability solution that additionally stores sessions in a [memcached](http://memcached.org/) compatible key-value store for session failover, while reading them from local memory for optimal performance (for sticky sessions). For non-sticky sessions the memcached compatible backend is used as session store (keeping a copy of the session in a secondary memcached node). "memcached compatible" here refers to the memcached protocol, therefore the backend can be any (if you like persistent) solution "speaking" memcached (e.g. [memcachedb](http://memcachedb.org/), [membase](http://membase.org/) etc.).

## Features ##
  * Supports Tomcat 6, Tomcat 7 and Tomcat 8
  * Handles sticky or non-sticky sessions
  * No Single Point of Failure
  * Handles tomcat failover
  * Handles memcached failover
  * Comes with pluggable session serialization
  * Allows asynchronous session storage for faster response times
  * Sessions are only sent to memcached if they're actually modified
  * JMX management & monitoring

## Which problem does the memcached-session-manager solve? ##

Imagine you have a web application with sticky sessions running on several tomcats and want to have some kind of session failover. You want to have a scalable solution for that – just add more servers to handle an increasing number of sessions. This can be handled by sessions that are stored for backup in memcached nodes: If a tomcat dies all other tomcats will take over the work of the lazy/dead one and fetch the sessions from the appropriate memcached node(s) and serve this session from thereon.

Wait. You're using non-sticky sessions? Since version 1.4.0 also non-sticky sessions are supported, with optional session locking for concurrent requests and without a single-point-of-failure (sessions are stored in a secondary memcached for backup).

## How does it work? ##
_(needs to be updated for non-sticky sessions)_

The memcached session manager installed in a tomcat holds all sessions locally in the own jvm, just like the [StandardManager](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/session/StandardManager.html) does it as well.

Additionally, after a request was finished, the session (only if existing) is additionally sent to a memcached node for backup.

When the next request for this session has to be served, the session is locally available and can be used, after this second request is finished the session is updated in the memcached node.

Now imagine the tomcat dies.

The next request will be routed to another tomcat. This tomcat (in more detail the memcached session manager) is asked for a session he does not know. He will now lookup the session in the memcached node (based on an id that was appended to the sessionId when the session was created). He will fetch the session from memcached and store the session locally in its own jvm - he is responsible for that session from now on. After the tomcat served this request of course he also updates the session in the memcached node. You see **tomcat failover** is handled completely.

And that was only the first part of the story.

## What else? ##
Also **memcached node failover** is implemented: if a memcached node is not available anymore the session will be moved to anoder node - also the sessionId will be modified and a new JSESSIONID cookie will be sent to the browser. As you are using sticky sessions make sure that the loadbalancer does use only the "plain" sessionId, without the suffix.

## How does the sessionId look like? ##
The memcached session manager knows a list of memcached nodes (e.g. as `n1.localhost:11211 n2.localhost:11212` that he references by the specified id (n1/n2). This id is encoded in the sessionId, so the sessionId might be `602F7397FBE4D9932E59A9D0E52FE178-n1`.

## What has been implemented/fixed with the latest releases? ##
Here's a complete list of [changes](http://code.google.com/p/memcached-session-manager/wiki/Changes).

## Is there a demo somewhere? ##
You want to see it in action? There's a [sample webapp on github](http://github.com/magro/msm-sample-webapp) that comes with two tomcats and a sample web application built with [wicket](http://wicket.apache.org/). The tomcats are configured to store sessions in memcached using kryo as serialization strategy. Just check out the project, mvn install the sample webapp and start memcached and the configured tomcats. [Start playing!](http://github.com/magro/msm-sample-webapp)