# 1.8.1 to 1.8.2 <font color='grey' size='3'>(2014-06-16)</font> #

  * [#208](http://code.google.com/p/memcached-session-manager/issues/detail?id=208) Add async support for tomcat8
  * [6873277a](https://github.com/magro/memcached-session-manager/commit/6873277a): Update library versions, specifically spymemcached to 2.11.1 and couchbase-client to 1.4.0 ([PR #41](https://github.com/magro/memcached-session-manager/pull/41) by Hans-Joachim Kliemeck)

# 1.8.0 to 1.8.1 <font color='grey' size='3'>(2014-02-02)</font> #

  * [#194](http://code.google.com/p/memcached-session-manager/issues/detail?id=194) Fix NoSuchMethodError in KryoTranscoderFactory on Tomcat 8 ([PR #38](https://github.com/magro/memcached-session-manager/pull/38) by Cyrille Le Clerc)

# 1.7.0 to 1.8.0 <font color='grey' size='3'>(2014-01-31)</font> #

  * [#173](http://code.google.com/p/memcached-session-manager/issues/detail?id=173): Support multiple contexts sharing the same session id (includes tomcats parallel deployment) - add `storageKeyPrefix` configuration attribute (see [SetupAndConfiguration](https://code.google.com/p/memcached-session-manager/wiki/SetupAndConfiguration#Overview_over_memcached-session-manager_configuration_attributes) for usage)
  * [87c19da](https://github.com/magro/memcached-session-manager/commit/87c19da): Rename kryo system property names `*.buffersize.*` to `*.bufferSize.*`
  * [f1f9f0a](https://github.com/magro/memcached-session-manager/commit/f1f9f0a): Create kryo default serializer via a factory method, configurable via the system property `msm.kryo.defaultSerializerFactory` ([PR #37](https://github.com/magro/memcached-session-manager/pull/37) by Marcus Thiesen), see [documentation](https://code.google.com/p/memcached-session-manager/wiki/SetupAndConfiguration#Overview_over_available_system_properties_(all_optional))
  * [83d9289](https://github.com/magro/memcached-session-manager/commit/83d9289): Delete Sessions which cause deserialization errors ([PR #36](https://github.com/magro/memcached-session-manager/pull/36) by Marcus Thiesen)
  * [#137](http://code.google.com/p/memcached-session-manager/issues/detail?id=137): Fixed "Found no validity info for session" warnings after invalidating session
  * [e160499](https://github.com/magro/memcached-session-manager/commit/e160499): Add samples that can be run with tomcat maven plugin, see [msm sample apps](https://github.com/magro/memcached-session-manager/tree/master/samples)
  * [#193](http://code.google.com/p/memcached-session-manager/issues/detail?id=193): Support Hibernate 4 in addition to Hibernate 3 in Kryo HibernateCollectionsSerializerFactory ([PR #34](https://github.com/magro/memcached-session-manager/pull/34) by Cyrille Le Clerc)
  * [#192](http://code.google.com/p/memcached-session-manager/issues/detail?id=192): After tomcat failover a session may be stored in a failoverNode while other nodes are available
  * [#190](http://code.google.com/p/memcached-session-manager/issues/detail?id=190): updateExpirationInMemcached prints negative memcachedExpirationTime
  * [#168](http://code.google.com/p/memcached-session-manager/issues/detail?id=168): Add tomcat8 support, thanx at Hans-Joachim Kliemeck and Cyrille Le Clerc for their initial contributions ([PR #28](https://github.com/magro/memcached-session-manager/pull/28), [PR #30](https://github.com/magro/memcached-session-manager/pull/30))

# 1.6.5 to 1.7.0 <font color='grey' size='3'>(2013-12-21)</font> #
  * [#185](http://code.google.com/p/memcached-session-manager/issues/detail?id=185): Locale is not serialized properly with jdk7
  * [#104](http://code.google.com/p/memcached-session-manager/issues/detail?id=104): Sessions expired in memcached with very high webapp session timeout (e.g. 1 year)
  * [8de1011a](https://github.com/magro/memcached-session-manager/commit/8de1011a): Make memcached client maxReconnectDelay configurable via sys prop "msm.maxReconnectDelay"
  * [73763b49](https://github.com/magro/memcached-session-manager/commit/73763b49): Make NodeAvailabilityCache TTL configurable (sys prop "msm.nodeAvailabilityCacheTTL"), increase default from 50 to 1000 millis
  * [#75](http://code.google.com/p/memcached-session-manager/issues/detail?id=75): Tomcat7: check/implement support of async/comet requests
  * [#177](http://code.google.com/p/memcached-session-manager/issues/detail?id=177): msm requires couchbase jars even when not using couchbase
  * [17c357df](https://github.com/magro/memcached-session-manager/commit/17c357df): Update spymemcached to 2.10.2 and couchbase-client to 1.2.2
  * [#145](http://code.google.com/p/memcached-session-manager/issues/detail?id=145): Add customConverter for Spring Security User class.
  * [#174](http://code.google.com/p/memcached-session-manager/issues/detail?id=174): sessions lost on Tomcat 7 reload
  * [#167](http://code.google.com/p/memcached-session-manager/issues/detail?id=167): Node failure handling fails due to spymemcached 2.7.3 to 2.8.12 update - use FailureMode.Redistribute for Couchbase ([PR #27](https://github.com/magro/memcached-session-manager/pull/27) by Hans-Joachim Kliemeck)

# 1.6.4 to 1.6.5 <font color='grey' size='3'>(2013-06-15)</font> #
  * [#153](http://code.google.com/p/memcached-session-manager/issues/detail?id=153): Name Threads in BackupSessionService
  * [#159](http://code.google.com/p/memcached-session-manager/issues/detail?id=159): Support context configured with cookies="false"
  * [f6492d10](https://github.com/magro/memcached-session-manager/commit/f6492d10) Updated tomcat versions to 6.0.37 and 7.0.40. ([PR #26](https://github.com/magro/memcached-session-manager/pull/26) by Hans-Joachim Kliemeck)
  * [d65ea3ff](https://github.com/magro/memcached-session-manager/commit/d65ea3ff) Support multiple couchbase configuration nodes ([PR #25](https://github.com/magro/memcached-session-manager/pull/25) by Hans-Joachim Kliemeck)
  * [891106d7](https://github.com/magro/memcached-session-manager/commit/891106d7) Support msm in parallel to context without msm: Additional check of requested context on host level ([PR #24](https://github.com/magro/memcached-session-manager/pull/24) by Hans-Joachim Kliemeck)
  * [86a215c4](https://github.com/magro/memcached-session-manager/commit/86a215c4) Support redeploys: Remove valve on shutdown, otherwise it will remain on the main container ([PR #24](https://github.com/magro/memcached-session-manager/pull/24) by Hans-Joachim Kliemeck)
  * Improved logging for issue [#166](http://code.google.com/p/memcached-session-manager/issues/detail?id=166) "Could not find session in local session map"

# 1.6.3 to 1.6.4 <font color='grey' size='3'>(2013-03-18)</font> #
  * [#126](http://code.google.com/p/memcached-session-manager/issues/detail?id=126): Switch to new CouchbaseClient API (by Hans-Joachim Kliemeck)
  * [095b6fe7](https://github.com/magro/memcached-session-manager/commit/095b6fe7) Fixed problem with detecting session ID if response contains multiple Set-Cookie headers. This patch works with both Tomcat6 and 7. (by Andrew McNair)
  * [#152](http://code.google.com/p/memcached-session-manager/issues/detail?id=152): Using msm tc6/7 in maven plugins like tomcat7-maven-plugin tomcat deps are pulled in (by Maciej Hryniszak)


# 1.6.2 to 1.6.3 #
  * [5bbb5ab5](https://github.com/magro/memcached-session-manager/commit/5bbb5ab5) Upgrade to kryo-serializers 0.10 with new DateSerializer (also the SubListSerializer was improved/fixed to work with java7)
  * [#148](http://code.google.com/p/memcached-session-manager/issues/detail?id=148): "java.io.IOException: Could not serialize object" - javolution serialization failed on windows7 (due to debug output)

# 1.6.1 to 1.6.2 #
  * [2d9eb930](https://github.com/magro/memcached-session-manager/commit/2d9eb930) When non-sticky session lock is released wait until the lock is removed in memcached
  * [#144](http://code.google.com/p/memcached-session-manager/issues/detail?id=144): When a non-sticky, locked session is expire()'ed the lock should be released
  * [#142](http://code.google.com/p/memcached-session-manager/issues/detail?id=142): Form based authentication/login not working with non-sticky sessions
  * [#141](http://code.google.com/p/memcached-session-manager/issues/detail?id=141): Non-sticky session should be loaded for ignored resources with container managed auth
  * [#140](http://code.google.com/p/memcached-session-manager/issues/detail?id=140): Improve lockingStrategy "auto": add HTTP method to url identification
  * [#138](http://code.google.com/p/memcached-session-manager/issues/detail?id=138): Fix support for ServletRequestListeners and tomcat 7.0.22+
  * [#129](http://code.google.com/p/memcached-session-manager/issues/detail?id=129): Non-sticky sessions: locking fails if operation timeout is high and long locks
  * [#124](http://code.google.com/p/memcached-session-manager/issues/detail?id=124): SASL not working with multiple memcached servers (thanx to Angelo Obreja)
  * [#122](http://code.google.com/p/memcached-session-manager/issues/detail?id=122): It is not necessay to loadFromMemcached in createSession
  * [#120](http://code.google.com/p/memcached-session-manager/issues/detail?id=120): Non-expiring, non-sticky sessions are always invalid when loaded from backup node
  * [77182d42](https://github.com/magro/memcached-session-manager/commit/77182d42) Use requestURI + queryString to check if uriIgnorePattern matches.

# 1.6.0 to 1.6.1 #
  * [#119](http://code.google.com/p/memcached-session-manager/issues/detail?id=119): Support locking with single node (without node-id) configuration

# 1.5.1 to 1.6.0 #
  * [#118](http://code.google.com/p/memcached-session-manager/issues/detail?id=118): Add configurable memcached operation timeout (contributed by Erik Swenson)
  * [#116](http://code.google.com/p/memcached-session-manager/issues/detail?id=116): Non-sticky sessions: Calling session.invalidate() causes "Found no validity info for session..." logging
  * [#115](http://code.google.com/p/memcached-session-manager/issues/detail?id=115): Support membase buckets (membase bucket uris) and SASL (initial contribution by Pradeep Gummi)
  * [#114](http://code.google.com/p/memcached-session-manager/issues/detail?id=114): msm-kyro-serializer 1.5.1 pom in maven central includes invalid dependencies
  * [#113](http://code.google.com/p/memcached-session-manager/issues/detail?id=113): Backup of a session should take place on the next available node when the next logical node is unavailable.
  * [#112](http://code.google.com/p/memcached-session-manager/issues/detail?id=112): No validity info stored for non-sticky sessions and memcachedNodes config without node-part (e.g. memcachedNodes="localhost:11211")
  * [#111](http://code.google.com/p/memcached-session-manager/issues/detail?id=111): Concurrent requests for a non-sticky session on the same tomcat may interfere
  * [#109](http://code.google.com/p/memcached-session-manager/issues/detail?id=109): Add flexjson serializer strategy (contribution by Sandeep More)
  * [#107](http://code.google.com/p/memcached-session-manager/issues/detail?id=107): Add custom kryo serializer for GrailsFlashScope

# 1.5.0 to 1.5.1 #
  * [#106](http://code.google.com/p/memcached-session-manager/issues/detail?id=106): Session not updated in memcached when only a session attribute was removed
  * [#105](http://code.google.com/p/memcached-session-manager/issues/detail?id=105): Make memcached node optional for single-node setup, so that the sessionId is left unchanged in this case (e.g. memcachedNodes="localhost:11211"). If instead memcachedNodes="n1:localhost:11211" is used the nodeId is still appended to the sessionId.

# 1.4.1 to 1.5.0 #
  * [#101](http://code.google.com/p/memcached-session-manager/issues/detail?id=101): Form based authentication does not work with non-sticky sessions
  * [#99](http://code.google.com/p/memcached-session-manager/issues/detail?id=99): Add attribute filter to control which session attributes are serialized to memcached (new configuration attribute `sessionAttributeFilter` accepting a regular expression, e.g. `sessionAttributeFilter="^(userName|sessionHistory)$"`). (thanx at Rainer Jung for this contribution!)
  * [#98](http://code.google.com/p/memcached-session-manager/issues/detail?id=98): Non-sticky sessions: Session meta data stored asynchronously with sessionBackupAsync="false"
  * [#97](http://code.google.com/p/memcached-session-manager/issues/detail?id=97): Add support for jsf2/mojarra kryo serialization (added custom serializer for mojarras LRUMap, this can be added as customConverter: `de.javakaffee.web.msm.serializer.kryo.FacesLRUMapRegistration`)
  * [#67](http://code.google.com/p/memcached-session-manager/issues/detail?id=67): Make msm available in maven repository (available in maven central now under groupId "de.javakaffee.msm"). So that both tomcat6 and tomcat7 versions can be pushed in one step to maven central there was a restructuring needed, so that now there's a core module (not specific to a tomcat version) and the tomcat6 and tomcat7 modules. Therefore for installation two msm jars have to be put in `$CATALINA_HOME/lib`: `memcached-session-manager-${version}.jar` and `memcached-session-manager-tc7-${version}.jar` (or `memcached-session-manager-tc6-${version}.jar`.

# 1.4.0 to 1.4.1 #

  * [#96](http://code.google.com/p/memcached-session-manager/issues/detail?id=96): Tests fail occasionally on windows (only changes to unit-test libs)
  * [#95](http://code.google.com/p/memcached-session-manager/issues/detail?id=95): Secondary session backup is tried to be removed with non-sticky sessions and only a single memcached
  * [#94](http://code.google.com/p/memcached-session-manager/issues/detail?id=94): Don't remove unserializable attributes from active sessions (with default/java serialization)
  * [#93](http://code.google.com/p/memcached-session-manager/issues/detail?id=93): Redundant retrieval of session from memcached after TC failover (with jvmRoute + securityConstraint/Valve)
  * [#92](http://code.google.com/p/memcached-session-manager/issues/detail?id=92): Incomplete change of session ID after Tomcat failover (with jvmRoute + securityConstraint/Valve)
  * [#91](http://code.google.com/p/memcached-session-manager/issues/detail?id=91): Windows only: memcached failover can break memcached client / connection
  * [#90](http://code.google.com/p/memcached-session-manager/issues/detail?id=90): When the jvmRoute contains a dash (-) sessions are not saved in memcached (tomcat7 only)
  * [#89](http://code.google.com/p/memcached-session-manager/issues/detail?id=89): NPE in SessionTrackerValve using log level DEBUG
  * [#88](http://code.google.com/p/memcached-session-manager/issues/detail?id=88): Support session-timeout of 0 or less (no session expiration)
  * [#87](http://code.google.com/p/memcached-session-manager/issues/detail?id=87): Tomcat doesn't start in TestUtils on Windows - backslash escape needed

# 1.4.0-RC3 to 1.4.0 #

  * [#86](http://code.google.com/p/memcached-session-manager/issues/detail?id=86): Don't load a non-sticky session from memcached when not accessed from request
  * [#85](http://code.google.com/p/memcached-session-manager/issues/detail?id=85): Don't perform additional tasks for non-sticky sessions with invalid sessionId
  * [#84](http://code.google.com/p/memcached-session-manager/issues/detail?id=84): Improve debug logging for non-sticky sessions and memcached failover
  * [#83](http://code.google.com/p/memcached-session-manager/issues/detail?id=83): Memcached node failover does not work for non-sticky sessions
  * [#76](http://code.google.com/p/memcached-session-manager/issues/detail?id=76): Nonsticky-session support: Check/implement listener notification on swapOut of sessions at the end of the request

# 1.4.0-RC2 to 1.4.0-RC3 #

  * [#82](http://code.google.com/p/memcached-session-manager/issues/detail?id=82): tomcat7: Manager not properly reinitialized on context reload
  * [#80](http://code.google.com/p/memcached-session-manager/issues/detail?id=80): Webapp context reload causes ClassCastException with sticky sessions
  * [#78](http://code.google.com/p/memcached-session-manager/issues/detail?id=78): Add JMX performance stats for non-sticky session operations

# 1.4.0-RC1 to 1.4.0-RC2 #

  * [#81](http://code.google.com/p/memcached-session-manager/issues/detail?id=81): Background expiration update for session that was loaded from memcached fails
  * [#79](http://code.google.com/p/memcached-session-manager/issues/detail?id=79): In non-sticky sessions mode with only a single memcached the backup is done in the primary node

# 1.3.6 to 1.4.0-RC1 #

[Announcement of release 1.4.0-RC1](http://groups.google.com/group/memcached-session-manager/t/612891b0ae10649d)

  * [#77](http://code.google.com/p/memcached-session-manager/issues/detail?id=77): Make memcached-session-manager work with Tomcat 7.x (thanx at Jorge Carabias for initial contribution)
  * [#70](http://code.google.com/p/memcached-session-manager/issues/detail?id=70): Improve support for nonsticky sessions
  * [#71](http://code.google.com/p/memcached-session-manager/issues/detail?id=71): Session locking for non-sticky sessions: all, auto detection of readonly requests, uri-pattern based
  * [#72](http://code.google.com/p/memcached-session-manager/issues/detail?id=72): Add storage of non-sticky sessions in another/secondary memcached node, handle memcached failover for non-sticky sessions
  * [#74](http://code.google.com/p/memcached-session-manager/issues/detail?id=74): Touching unmodified/not-updated sessions must be solved differently
  * [#68](http://code.google.com/p/memcached-session-manager/issues/detail?id=68): Change of sessionId on login with BASIC auth must be handled correctly
  * [#66](http://code.google.com/p/memcached-session-manager/issues/detail?id=66): Make SessionTrackerValve.setNewSessionCookie more robust (handle different possible cases)
  * [#65](http://code.google.com/p/memcached-session-manager/issues/detail?id=65): Session cookie name is hardcoded to JSESSIONID
  * [#62](http://code.google.com/p/memcached-session-manager/issues/detail?id=62): Support jvmRoutes containing dashes (-) (thanx at Paul G. Weiss for patches)
  * [#60](http://code.google.com/p/memcached-session-manager/issues/detail?id=60): Add possibility to disable msm at runtime via JMX (thanx at Paul G. Weiss for patches)

# 1.3.0 to 1.3.6 #

[Announcement of release 1.3.6](http://groups.google.com/group/memcached-session-manager/t/28540c1feec4b273)

  * Added new serialization strategy based on [kryo](http://code.google.com/p/kryo/) with a really good performance: SerializationStrategyBenchmark (usage and configuration described in SetupAndConfiguration).
  * A new `DummyMemcachedBackupSessionManager` for permanent failover simulation during development, to see that session deserialization is working (more is described on SetupAndConfiguration with the `className` attribute).
  * Removed msm-javolution-serializer-cglib and msm-javolution-serializer-jodatime, both custom converters are now included in the msm-javolution-serializer jar.
  * Added custom converter for serialization/deserialization of hibernate persistent collections (for javolution and kryo), details on SetupAndConfiguration.

# 1.2.0 to 1.3.0 #

[Announcement of release 1.3.0](http://groups.google.com/group/memcached-session-manager/t/1f347fbb626338fc)

Change:
  * [#53](http://code.google.com/p/memcached-session-manager/issues/detail?id=53): By default, perform session backup asynchronously (change default for sessionBackupAsync from false to true).

Improvements:
  * [#50](http://code.google.com/p/memcached-session-manager/issues/detail?id=50): Allow configuration of memcached protocol to use
  * [#51](http://code.google.com/p/memcached-session-manager/issues/detail?id=51): Session serialization should be done asynchronously when sessionBackupAsync=true
  * [#52](http://code.google.com/p/memcached-session-manager/issues/detail?id=52): Add backupThreadCount attribute to specify number of threads to use for async session backup
  * [#54](http://code.google.com/p/memcached-session-manager/issues/detail?id=54): Support session cookie 'HttpOnly' flag when changing session id due to memcached failover (for tomcat >= 6.0.19 only)
  * [#56](http://code.google.com/p/memcached-session-manager/issues/detail?id=56): javolution-serializer: support public Collections/Maps providing only a constructor taking an int (size)
  * [#57](http://code.google.com/p/memcached-session-manager/issues/detail?id=57): javolution-serializer: Provide more efficient serialization for StringBuffer and StringBuilder
  * [#59](http://code.google.com/p/memcached-session-manager/issues/detail?id=59): Add support for serialization/deserialization of cglib proxies to javolution-serializer

Fixes:
  * [#38](http://code.google.com/p/memcached-session-manager/issues/detail?id=38): Notify HttpSessionListeners and HttpSessionActivationListeners when loading a session from memcached
  * [#55](http://code.google.com/p/memcached-session-manager/issues/detail?id=55): Wicket's MiniMap cannot be deserialized with javolution serializer
  * [#58](http://code.google.com/p/memcached-session-manager/issues/detail?id=58): Change jvmRoute on tomcat failover (when a session is served by another tomcat)


# 1.1.1 to 1.2.0 #

[Announcement of release 1.2.0](http://groups.google.com/group/memcached-session-manager/t/f113b97a121d1e9f)

Improvements:
  * There's a new configuration attribute "customConverter" that provides the possibility to specify custom converter. This is only supported by the javoluation serializer (not possible for java and xstream serialization)
  * [#32](http://code.google.com/p/memcached-session-manager/issues/detail?id=32): msm-javolution-serializer should provide more efficient serialization of joda DateTime (this is available as msm-javolution-serializer-jodatime, required the "customConverter" feature)
  * [#36](http://code.google.com/p/memcached-session-manager/issues/detail?id=36): Store modified sessions only
  * [#42](http://code.google.com/p/memcached-session-manager/issues/detail?id=42): createSession might take a possibly provided sessionId into account
  * [#43](http://code.google.com/p/memcached-session-manager/issues/detail?id=43): Calculate statistics for serialization time / backup time / load time / skips etc (see [2](2.md) for more)
  * [#44](http://code.google.com/p/memcached-session-manager/issues/detail?id=44): Support comma-separated list of nodes
  * [#46](http://code.google.com/p/memcached-session-manager/issues/detail?id=46): Support reconfiguration of memcached nodes at runtime via jmx

Fixes:
  * [#33](http://code.google.com/p/memcached-session-manager/issues/detail?id=33): msm-javolution-serializer: ReflectionBinding does not honor XMLSerializable interface
  * [#34](http://code.google.com/p/memcached-session-manager/issues/detail?id=34): msm-javolution-serializer: java.util.Currency gets deserialized with ReflectionFormat
  * [#35](http://code.google.com/p/memcached-session-manager/issues/detail?id=35): Use same logging system as Tomcat
  * [#40](http://code.google.com/p/memcached-session-manager/issues/detail?id=40): webapp sets cookie for every page if memcached process is missing
  * [#41](http://code.google.com/p/memcached-session-manager/issues/detail?id=41): createSession should take maxActiveSessions into account
  * [#47](http://code.google.com/p/memcached-session-manager/issues/detail?id=47): Cookie set on memcached failure doesn't honor request's secure property
  * [#48](http://code.google.com/p/memcached-session-manager/issues/detail?id=48): Memcached failover / session relocation not supported when cookies are disabled
  * [#49](http://code.google.com/p/memcached-session-manager/issues/detail?id=49): Sessions not associated with a memcached node don't get associated as soon as a memcached is available


# 1.1 to 1.1.1 (only msm-javolution-serializer) #
This refers to the maintenance release 1.1.1 for the msm-javolution-serializer with the following fixed bugs:
  * [#27](http://code.google.com/p/memcached-session-manager/issues/detail?id=27): Serialization of AtomicInteger fails for msm-javolution-serializer
  * [#28](http://code.google.com/p/memcached-session-manager/issues/detail?id=28): msm-javolution-serializer should support serialization of java.util.Collections$EmptyList
  * [#30](http://code.google.com/p/memcached-session-manager/issues/detail?id=30): msm-javolution-serializer should support serialization of java.util.Collections$UnmodifiableMap
  * [#31](http://code.google.com/p/memcached-session-manager/issues/detail?id=31): msm-javolution-serializer should be able to load classes from webapp context

# 1.0 to 1.1 #
  * Added pluggable session serialization strategies (see SerializationStrategies), configurable via the configuration attribute _transcoderFactoryClass_ (described on [SetupAndConfiguration](SetupAndConfiguration#MemcachedBackupSessionManager_configuration_attributes.md)
  * Added XStream based serialization strategy (see SerializationStrategies)
  * Added Javolution based serialization strategy (see SerializationStrategies)
  * Added configuration attribute _copyCollectionsForSerialization_ (see [SetupAndConfiguration](SetupAndConfiguration#MemcachedBackupSessionManager_configuration_attributes.md) and [SerializationStrategies#Concurrent\_modifications\_of\_collections](SerializationStrategies#Concurrent_modifications_of_collections.md))

# 1.0 #
  * Initial release of msm