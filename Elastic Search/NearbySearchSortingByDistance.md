# Nearby Search Sorting by Distance

To get the nearby geo points given a requested (lat, lng), I am going to combine 
[FilterBuilder](http://rajish.github.io/api/elasticsearch/0.20.0.Beta1-SNAPSHOT/org/elasticsearch/index/query/FilterBuilder.html) to get nearby geo destinations
and
[SortBuilder](http://rajish.github.io/api/elasticsearch/0.20.0.Beta1-SNAPSHOT/org/elasticsearch/search/sort/SortBuilder.html) to sort geo results by distance


Code snippet looks like following:

```java
final FilterBuilder filterBuilder 
    = FilterBuilders.geoDistanceFilter("location_field_name")
        .point(latitude, longitude)
        .distance(distance, distanceUnit)
        .optimizeBbox("memory");

final SortBuilder sortBuilder
    = SortBuilders.geoDistanceSort("location_field_name")
        .point(latitude, longitude)
        .unit(distanceUnit)
        .order(SortOrder.ASC);

final SearchResponse response
    = client.prepareSearch("index")
        .setTypes("type")
        .setPostFilter(filterBuilder)
        .addSort(sortBuilder)
        .setSize(resultSize)
        .execute()
        .actionGet();
```

This will return `resultSize` geo locations that are closest geo locations nearby given (lat, lng) and results are sorted by distance order.