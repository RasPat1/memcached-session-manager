This page shows the results of the [serializer-benchmark](http://github.com/magro/memcached-session-manager/tree/master/serializer-benchmark/) that compares java, javolution based and kryo based serialization/deserialization times for a sample object graph (some persons, addresses, calendar objects etc.).

The benchmark was performed on my T61 (Core Duo T7300, 2x2GHz) running Fedora Core 12 with Sun JDK 1.6.0\_18-b07:
```
java -Xms512M -Xmx512M -classpath [..] de.javakaffee.web.msm.serializer.Benchmark
```

The plain numbers are published here: [spreadsheets.google.com](http://spreadsheets.google.com/pub?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&gid=0) (all number are millis, also in the charts below)

A short explanation of the labels:
  * _Ser_ - serialization time
  * _Deser_ - deserialization time (you guessed it :-))
  * _Min_, _Avg_, _Max_ - minimum, average and maximum time in millis required for 500 serializations/deserializations (10 cycles)


### Benchmark small (10 person samples, 7 component samples) ###
![http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=12&v=1273186036253&foo=bar.png](http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=12&v=1273186036253&foo=bar.png)


### Benchmark medium (100 person samples, 31 component samples) ###
![http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=10&v=1273186069983&foo=bar.png](http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=10&v=1273186069983&foo=bar.png)


### Benchmark big (500 person samples, 261 component samples) ###
![http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=8&v=1273186101696&foo=bar.png](http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddDRWeEl3LWs5R0UxbFB6MURmdHY1cGc&oid=8&v=1273186101696&foo=bar.png)


### Serialized data size ###
The following chart shows the size of the serialized data for the different serialization strategies for 10, 100, 500 samples (here's the [spreadsheet with numbers](https://spreadsheets.google.com/pub?key=0AspDnQzTrkhddFN5ZUVfTEhLRTVCTi00ZkJiWldteXc)):

[![](http://spreadsheets.google.com/oimg?key=0AspDnQzTrkhddFN5ZUVfTEhLRTVCTi00ZkJiWldteXc&oid=2&foo=bar.png)](https://spreadsheets.google.com/pub?key=0AspDnQzTrkhddFN5ZUVfTEhLRTVCTi00ZkJiWldteXc)