#  Streamr Integeration library for Apache FLink
## How to use

Add this to your pom-xml file:
```
<dependency>
  <groupId>com.streamr.labs</groupId>
  <artifactId>streamr_flink</artifactId>
  <version>0.1</version>
</dependency>
```

Import the Streamr subscriber or publisher for Flink with:
```
import com.streamr.labs.streamr_flink.StreamrPublish;
import com.streamr.labs.streamr_flink.StreamrSubscribe;
```

Example code:
``` java
final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
StreamrSubscribe streamrSub = new StreamrSubscribe("YOUR_STREAMR_API_KEY", "YOUR_SUB_STREAM_ID");
StreamrPublish streamPub = new StreamrPublish("YOUR_STREAMR_APIKEY", "YOUR_PUB_STREAM_ID");

DataStreamSource<Map<String, Object>> tramDataSource = env.addSource(streamrSub);
// Filter the 6T trams.
DataStream<Map<String, Object>> stream = tramDataSource.filter(new FilterFunction<Map<String, Object>>() {
  @Override
  public boolean filter(Map<String, Object> s) throws Exception {
    return (s.containsKey("desi") && s.get("desi").toString().contains("6"));
  }
});
// Add the publish sink
stream.print();
stream.addSink(streamPub);

env.execute("Trams");
```

For more info go to https://github.com/streamr-dev/streamr-apache-flink-integration