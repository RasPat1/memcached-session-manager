# Introduction #
By default memcached-session-manager uses Java's serialization to serialize session data so that it can be sent to memcached. The same applies to deserialization of course.

As Java's serialization has some limitations/disadvantages, the serialization strategy can be changed and replaced by alternative ones. However, Java serialization also brings in some useful stuff, which should not get lost with other serialization strategies.

There's a [performance comparison](SerializationStrategyBenchmark.md) of some of the provided serialization strategies.


# Features of serialization strategies #
The following features should be provided by all serialization strategies, as they are related to correct behavior of either the serialization-/deserialization process or the deserialized objects compared to the original ones.

  * **Serialization: Handle cyclic dependencies.** Cyclic dependencies may occur in an object graph and therefor should be supported. The solution is to additionally store ids for serialized objects and refids to already serialized objects if it's found again.
  * **Serialization/Deserialization: Support references to a shared object.** If two instances A and B share the same reference to a shared object C this must be the same for the deserialized objects. As the solution is the same as for cyclic dependencies, both features are fairly related.
  * **Deserialization: Support private classes.** During deserialization private classes are a challange, as even the Class object is not visible and therefore you cannot dynamically create an instance of such a class.
  * **Deserialization: Support classes without default constructor.** If a class does not provide a default constructor (a constructor without any arguments), you don't know how to create a new instance of this class.

### Concurrent modifications of collections ###
A special feature that can be activated with the _copyCollectionsForSerialization_ configuration property targets the case, that there are multiple requests running in parallel for the same session (e.g. because of AJAX) and your application uses non-thread-safe collections like `java.util.ArrayList` or `java.util.HashMap`. In this case, your application might modify such a collection while it's being serialized for backup in memcached, which leads to `ConcurrentModificationException`s during serialization (of course this can happen in your application as well).
The preferred way to support multiple parallel requests for the same session should be to use thread-safe collections, but there might be situations where you cannot provide this. If you're in this situation you might consider the _copyCollectionsForSerialization_ feature (note that this is currently only supported by the javolution based serialization strategy!).

# Available serialization strategies #
This is a list of available serialization strategies and their features.

| **Serialization Strategy** <br /> _Value for `transcoderFactoryClass` attribute_ | **Requires**<br /> `java.io.Serializable`| **Cyclic**<br /> Dependencies| **Shared**<br /> objects| **Private classes** | **Classes without**<br /> default constructor| **Different class versions** | **Copy Collections**<br /> before <br /> serialization| **Custom**<br /> Converter| **Comment** |
|:---------------------------------------------------------------------------------|:-----------------------------------------|:-----------------------------|:------------------------|:--------------------|:---------------------------------------------|:-----------------------------|:------------------------------------------------------|:--------------------------|:------------|
| **java serialization** (default, bundled with msm) <br /> `de.javakaffee.web.msm.JavaSerializationTranscoderFactory` | Yes | Yes | Yes | Yes | Yes | No (Though, if the serialVersionUID is set to 1L, classes can be deserialized even if the new class version has new fields) | No | No |  |
| **msm-kryo-serializer** <br /> `de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFactory` | No | Yes | Yes | Yes (for Sun JVMs) | Yes (for Sun JVMs) | No (not yet) | Yes | Yes (Converter must extend `KryoCustomization`, `SerializerFactory` or `UnregisteredClassHandler`) | Reflection based, [Kryo](http://code.google.com/p/kryo/) is used for binary serialization/deserialization |
| **msm-javolution-serializer** <br /> `de.javakaffee.web.msm.serializer.javolution.JavolutionTranscoderFactory` | No | Yes | Yes | Yes (for Sun JVMs) | Yes (for Sun JVMs) | Yes (During deserialization, fields that are not existing in a class are ignored) | Yes | Yes (Converter must extend [apidocs/javolution/xml/CustomXMLFormat.html CustomXMLFormat]) | Reflection based, [Javolution](http://javolution.org/) is used for actual xml encoding/decoding, it also does the object reference handling |
| **msm-xstream-serializer** <br /> `de.javakaffee.web.msm.serializer.xstream.XStreamTranscoderFactory` | No | Yes | Yes | Yes (see also [XStream FAQ](http://xstream.codehaus.org/faq.html#Compatibility_JVMs)) | Yes (see also [XStream FAQ](http://xstream.codehaus.org/faq.html#Compatibility_JVMs)) | Yes (see also [XStream FAQ](http://xstream.codehaus.org/faq.html#Serialization_newer_class_versions)) | No | No | [XStream](http://xstream.codehaus.org/) does all the work |
| **msm-flexjson-serializer** <br /> `de.javakaffee.web.msm.serializer.json.JSONTranscoderFactory` | No | Yes | Yes | Yes | No | No | No | No | [flexjson](http://flexjson.sourceforge.net/) is used for serialization. See also [#109](http://code.google.com/p/memcached-session-manager/issues/detail?id=109) |

My personal opinion regarding these different strategies:
  * Java serialization is very robust and a proven technology. The biggest disadvantage IMHO is that different class versions cannot be handled.
  * [Kryo](http://code.google.com/p/kryo/) is an extremely fast binary serialization library. In the popular [thrift-protobuf-compare](http://code.google.com/p/thrift-protobuf-compare/wiki/Benchmarking) benchmark it's one of the fastest serialization toolkits - and it differs from the fastest in that it does **NOT** need a schema definition of serialized data, which is a requirement for serialization arbitrary session data. A disadvantage of using kryo based serialization is that it's binary - you just cannot look how the serialized object graph looks like. This is my favorite serialization strategy, just because of its [great performance](SerializationStrategyBenchmark.md).
  * [Javolution](http://javolution.org/) is a very good and fast xml binding toolkit. The reflection part is written by me and adds the bits that are actually binding POJOs to xml. It is covered well with unit tests, however I cannot guarantee that there's no issue left to solve (actually this serialization strategy was in use in my own projects, now replaced by kryo based serialization).
  * [XStream](http://xstream.codehaus.org/) based serialization should be very robust as  this is an often used java object binding library. The biggest disadvantage IMHO is the relatively bad performance.

A rough performance comparison of some of the serialization strategies can be found [here](SerializationStrategyBenchmark.md), an older one without kryo but including xstream can be found [here](Performance.md).

# Want to roll your own? #
This section shall outline, which feature a serialization protocol should provide so that it can be used for a serialization strategy for the memcached-session-manager. You should check this if you want to create another serialization strategy.


  * **Serialization of dynamic structures.** This requirement is a result from an asumption, that users shall not be forced to provide s.th. like a schema for their session objects. Most of the time, users even don't know really, what gets stored in the http session and what not. Therefore it's rather impossible to provide a structural definition of the session attributes. However, if this asumption is wrong, serialization of dynamic structures is no longer a requirement :)
  * **Recreate appropriate types during deserialization.** As session attributes are just bound to a name, that type information for the attribute values must be stored by the serialization protocal. That's the reason, why JSON is (unfortunately) not an appropriate serialization protocol for the memcached-session-manager. One would need to implement a custom JSON format (and I would be happy, if Jackson would/could do this).