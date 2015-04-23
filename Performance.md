# Introduction #

This page shall show performance characteristics of the memcached-session-manager (msm). For this there's a simple web application, which can be run with different msm configurations including different session serialization strategies. Additionally this web application shall show, how performance depends on session size.


# Details #

The web application is written in java, making use of [wicket](http://wicket.apache.org). It provides several pages:
  * StatelessPage - a simple page without any state.
  * SessionPage - a page without real wicket state, but it creates an http session.
  * StatefullPage - a page with state, you can click and increment a counter.
  * ListLightPage - a page with state, showing a list of items for which their ids are stored in the session.

For performance testing I chose [ab](http://httpd.apache.org/docs/2.0/programs/ab.html).

## Comparing throughput ##

In the following, the number of requests/sec is compared for different configurations:
  * no msm configured, just in-memory sessions
  * msm with java serialization
  * msm with xstream serialization

For each these configurations the number of requests/second is measured for the mentioned pages above (stateless, session etc.).

Tests were running on my local laptop, both memcached and tomcat, and also ab were running on the same machine. One might run the tests with different setups to see how numbers change.

ab was executed using the following parameters (in this case for the stateless page):
```
ab -n 10000 -c 200 http://localhost:8080/stateless
```

(10000 requests, 200 concurrent)

![http://chart.apis.google.com/chart?cht=bhg&chdl=no%20msm|msm(javolution)|msm(java)|msm(xstream)&chco=4A3BBE,6073ED,90ABF2,B4C6E4&chdlp=b&chtt=requests/s%20(the%20more%20the%20better)&chs=680x360&chbh=a&chds=0,2037.79&chd=t:2037.79,1506.34,919.53,325.8|2037.65,1310.49,712.52,191.08|2037.75,1270.6,670.99,214.03|2035.08,1172.03,288.24,51.91&chxt=y,x&chxl=0:|listlight|statefull|session|stateless&chxr=1,0,2000&chm=N,404040,0,-1,10,1|N,404040,1,-1,10,1|N,404040,2,-1,10,1|N,404040,3,-1,10,1&chg=10,0,1,5&foo=bar.png](http://chart.apis.google.com/chart?cht=bhg&chdl=no%20msm|msm(javolution)|msm(java)|msm(xstream)&chco=4A3BBE,6073ED,90ABF2,B4C6E4&chdlp=b&chtt=requests/s%20(the%20more%20the%20better)&chs=680x360&chbh=a&chds=0,2037.79&chd=t:2037.79,1506.34,919.53,325.8|2037.65,1310.49,712.52,191.08|2037.75,1270.6,670.99,214.03|2035.08,1172.03,288.24,51.91&chxt=y,x&chxl=0:|listlight|statefull|session|stateless&chxr=1,0,2000&chm=N,404040,0,-1,10,1|N,404040,1,-1,10,1|N,404040,2,-1,10,1|N,404040,3,-1,10,1&chg=10,0,1,5&foo=bar.png)

One thing that this chart shows, is that for stateless pages/requests it makes no difference if you have msm integrated or not.

For sessions, it depends on the serialization strategy.

Java serialization is faster than xstream (running with xpp).The advantage of xstream is, that it can serialize even classes that do not implement `java.io.Serializable`.

### Serialized Content ###
Here you find how XStream had serialized the different pages:

  * [session-xstream.xml](http://memcached-session-manager.googlecode.com/svn/wiki/Performance.attach/session-xstream.xml)
  * [statefull-xstream.xml](http://memcached-session-manager.googlecode.com/svn/wiki/Performance.attach/statefull-xstream.xml)
  * [listlight-xstream.xml](http://memcached-session-manager.googlecode.com/svn/wiki/Performance.attach/listlight-xstream.xml)