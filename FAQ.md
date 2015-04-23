### How are sessions stored in memcached in non-sticky mode? ###
For non-sticky sessions one of the nodes is randomly elected as primary
node (the nodeId is encoded in the sessionId). The logical "next" node
is chosen as backup node. The session is stored in the primary node and
additionally in the backup node (under a key "`bak:<sessionId>`").
On further requests the session is updated in both primary and backup
nodes, if it wasn't modified the session is only "pinged" in memcached
(to prevent expiration).

### What happens in non-sticky mode when a memcached node fails? ###
a) Primary node fails: When the primary node is not available for a
request (so that the session cannot be loaded from the primary node), it
will be pulled from the backup node and the backup node will become the
primary node. When the backup for the session shall be saved, the backup
node will be the next node relative to the new primary one. As an
example, with nodes n1, n2, n3: first n2 would be primary and n3 backup
node. When n2 fails n3 will become the primary and n1 will be used as
backup node.

b) Backup node fails: When the backup node fails the backup will be skipped.

### I have several (sticky) tomcats and memcached nodes, how shall I configure `failoverNodes` for each tomcat? ###
`failoverNodes` are for setups where some tomcats and memcacheds are
running on the same machine. When the machine serving a tomcat crashes
the session can only be served by another tomcat when the session is
stored in a memcached running on a different machine.
So a tomcat shall write sessions preferrably to memcacheds running on
other machines, and store sessions only in a memcached running on the
same machine when no other memcached is available. That's the meaning of
failoverNodes.

Some examples:

  1. Example
    * machines m1, m2
    * tomcats t1, t2 on m1, t3, t4 on m2
    * memcached nodes n1 on m1, n2 on m2
    * -> `failoverNodes` for t1 and t2 = n1, `failoverNodes` for t3 and t4 = n2
  1. Example
    * machines m1, m2, m3, m4
    * tomcats t1 on m1, t2 on m2, t3 on m3 and t4 on m4
    * memcaches n1 on m1, n2 on m2, n3 on m3, n4 on m4
    * -> t1.`failoverNodes` = n1, t2.`failoverNodes` = n2, t3.`failoverNodes` = n3, t4.`failoverNodes` = n4
  1. Example
    * machines m1, m2, m3, m4
    * tomcats t1 on m1, t2 on m2
    * memcacheds n1 on m3, n2 on m4
    * -> t1.`failoverNodes` and t2.`failoverNodes` = `<empty>` (not needed in this case)


### Can I just use a servlet filter (& friends) instead? ###
(Extract from the mailing list, "[memcached session integration in application layer](http://groups.google.com/group/memcached-session-manager/browse_thread/thread/3630849929d81295)")

Basically it's possible to use a simple servlet filter etc. for keeping sessions in memcached (or membase etc.), but there are some things to be aware of and that must be checked against your requirements.

Assuming that sticky sessions shall be used the main issue is sessionId handling: the servlet api does not allow you to specify/modify the sessionId. So you cannot encode the memcached-node of the session into the sessionId, and have to use hash based memcached node selection (be it consistent hashing or not). If a memcached fails, you can use failover handling of the memcached client lib of your choice
and see if this matches your requirements.
If you want to go for the servlet filter with sticky sessions, you also need to strip the jvmRoute off the sessionId to get the memcached key (in the case tomcats are configured with a jvmRoute). Otherwise, when the sessionId including the jvmRoute would be used as memcached key, the key would change on a tomcat failover as the jvmRoute is then changed to the one of the tomcat that is taking over the session.

With non-sticky sessions there are some more things to be aware of: as you want to have no sessions in tomcat stored at all you have to manager sessionIds yourselves. This includes url rewriting and cookie handling. Also you have to handle concurrent session access from different app servers (introduced by tabbed browsing or ajax etc.). Probably there are some more details to be aware of, but these are the main issues that are already covered by memcached-session-manager.

### Why is it not implemented as [Store](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/Store.html) for tomcats [PersistentManagerBase](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/session/PersistentManagerBase.html)? ###

The [Store](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/Store.html) interface defines methods _getSize()_, _keys()_ and _clear()_ - these cannot be implemented with memcached.

Also [PersistentManagerBase](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/session/PersistentManagerBase.html) backups all sessions in batches (_processMaxIdleBackups()_ is called by the background thread after active sessions have been checked for expiration). If each session shall be up to date in the store, all sessions would be sent to memcached again and again.

The memcached-session-manager instead backups a session after the request was finished, so only sessions that were (potentially) modified are sent to memcached.

### How does msm compare to tomcats DeltaManager or BackupManager? ###
The `DeltaManager` does an all-to-all replication which limits scalability.
The `BackupManager` (according to the docs [not quite as battle tested as the delta manager](http://tomcat.apache.org/tomcat-7.0-doc/cluster-howto.html)) does a replication to another tomcat, which requires special configuration in the load balancer to support this.

memcached-session-manager supports non-sticky sessions (not supported by Delta/BackupManager) and provides session locking to handle concurrent requests served by different tomcats.

DeltaManager/BackupManager use java serialization, memcached-session-manager comes with pluggable serialization strategies and e.g. one implementation that is based on [kryo](http://code.google.com/p/kryo/), one of the [fastest serialization libs](https://github.com/eishay/jvm-serializers/wiki/Staging-Results) as of today (2012).

### How are memcached nodes selected for session backup? ###
When a new session is created the memcached-session-manager selects the memcached node randomly.

### What's the format of the session id when using the memcached-session-manager? ###
The memcached node is encoded in the session id like this: `<sessionId>-<node>[.<jvmRoute>]` (e.g. `602F7397FBE4D9932E59A9D0E52FE178-n1` without a jvm route, with the jvm route `602F7397FBE4D9932E59A9D0E52FE178-n1.tomcat0`)