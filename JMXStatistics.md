# Introduction #

With version 1.2 there are runtime statistics that are provided via jmx. Statistics are enabled by default, but you can also disable them via _enableStatistics="false"_ in the manager configuration.

To access the gathered stats just start jconsole (or visualvm, or what else your preferred jmx tool is), open the MBeans tab and browse to the Manager node, the appropriate Context and the correct Hostname (e.g. _Manager -> /myapp -> localhost_). Open the attributes view and look for everything starting with _msmStat_. Details regarding the meaning of each attribute are provided below.


# Details #

The following statistics attributes are provided by the memcached-session-manager via jmx. If you're interested in more just [submit an issue](http://code.google.com/p/memcached-session-manager/issues/list).

| **Attribute Name** | **Description** |
|:-------------------|:----------------|
| msmStatNumRequestsWithoutSession | The number of requests without a session |
| msmStatNumRequestsWithSession | The number of requests with a session |
| msmStatNumTomcatFailover | The number of sessions that were taken over from other tomcats |
| msmStatNumMemcachedFailover | The number of session relocations (how many times sessions were moved to another memcached node) |
| msmStatNumBackupFailures | The number of backup failures (sessions could not be stored in any memcached node) |
| msmStatNumNoSessionAccess | The number of requests that did not access the session at all (so that session backup was skipped) |
| msmStatNumNoAttributesAccess | The number of requests that did access the session but did not access session attributes via getAttribute/setAttribute (so that session backup was skipped, and it was not necessary to determine, if the session was modified) |
| msmStatNumNoSessionModification | The number of requests that did access the session but did not change any session attribute (so that session backup was skipped) |
| msmStatEffectiveBackupInfo | Count, min, avg and max of the total time in millis for session backup spent in the request thread (measured for all requests with a session), including requests where session backup could be omitted e.g. because the session attributes were not accessed. |
| msmStatBackupInfo | Count, min, avg and max of the total time in millis for session backup, including serialization and sending data to memcached |
| msmStatAttributesSerializationInfo |  Count, min, avg and max of the time in millis for session serialization as part of the backup (actually just the time for the serialization of session attributes) |
| msmStatMemcachedUpdateInfo | Count, min, avg and max of the time in millis for storing data in memcached as part of the backup (excluding serialization, including compression) |
| msmStatCachedDataSizeInfo | Count, min, avg and max of the size of the data that was sent to memcached (the serialized data after compression) |
| msmStatSessionsLoadedFromMemcachedInfo | Count, min, avg and max of the time in millis that took loading sessions from memcached (including deserialization) |
| msmStatSessionDeserializationInfo |  Count, min, avg and max of the time in millis spent for session deserialization when loading a session from memcached |
| msmStatSessionsDeletedFromMemcachedInfo |  Count, min, avg and max of the time in millis spent for deleting a session because a session expired or it was taken over from another tomcat (tomcat failover) |

## Statistics related to non-sticky sessions ##

The following attributes are visible for both sticky and non-sticky sessions but are only relevant for non-sticky sessions.

| **Attribute Name** | **Description** |
|:-------------------|:----------------|
| msmStatNonStickyAfterLoadFromMemcachedInfo | Count, min, avg and max of the time spent in the request thread after a non-sticky session was loaded from memcached (session metadata is loaded) |
| msmStatNonStickyAfterBackupInfo | Count, min, avg and max of the time spent in the request thread after the session storage for storing separate metadata (and starting a thread for the additional backup of metadata and session data in a secondary memcached) |
| msmStatNonStickyOnBackupWithoutLoadedSessionInfo | Count, min, avg and max of the time spent (in the request thread) for requests that did not access the session (metadata is loaded/updated, a thread is started for ping of session and 2ndary session backup and for the backup of metadata in secondary memcached). |
| msmStatNumNonStickySessionsPingFailed | How often a non-sticky session got pinged and the session was no longer existing |
| msmStatNonStickyAfterDeleteFromMemcachedInfo | Count, min, avg and max of the time spent in the request thread after a session was deleted from memcached (e.g. due to session expiration) |
| msmStatNonStickyStoreMetaDataInfo | -- _not used, will be removed in future version_ -- |
| msmStatNonStickyAcquireLockInfo | Count, min, avg and max of the time spent in the request thread for session lock acquiration |
| msmStatNonStickyAcquireLockFailureInfo | Count, min, avg and max of the time spent in the request thread for session lock acquiration that failed with a timeout |
| msmStatNonStickyReleaseLockInfo | Count, min, avg and max of the time spent for releasing a previously acquired session lock |
| msmStatNumNonStickySessionsReadOnlyRequest | For locking strategy "auto": the number of requests that were considered to be readonly so that no session lock was acquired |