---
layout: post
title:  "Welcome to Jackson!"
date:   2021-01-16 14:10:00 -0700
categories: jackson json
---
### Simple serialization/deserialization via ObjectMapper

{% highlight java %}
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(cat);
Cat cat = mapper.readValue(json, Cat.class);
{% endhighlight %}

### What fields will be serialized/deserialized?
- public fields 
- if getter is available (serializable and deserializable)
- if setter is available (only deserializable)
- apply `@JsonAutoDetect` (for example, `@JsonAutoDetect(fieldVisibility = Visibility.ANY)` )

`@JsonIgnore` can be added to a field itself or to getter/setter.

Jackson requires either no-arg constructor or @JsonCreator. The latter works for custom deserialization.
{% highlight java %}
@JsonCreator
public ImportResultItemImpl(@JsonProperty("name") String name, 
        @JsonProperty("resultType") ImportResultItemType resultType, 
        @JsonProperty("message") String message) {
    super();
    this.resultType = resultType;
    this.message = message;
    this.name = name;
}
{% endhighlight %}

Note: won't work for non-static inner class <http://www.cowtowncoder.com/blog/archives/2010/08/entry_411.html>

### Rename fields, set required flag

- `@JsonProperty` is for a field
- `@JsonGetter` / `@JsonSetter` are for getter/setter.
- `@JsonAlias` - several possible names for one field

{% highlight java %}
@JsonProperty(
    value="Value",
    required=true,
    defaultValue="Empty",
    access= Access.READ_WRITE)
public String _value; 
  
@JsonGetter("name")
public String getTheName() {
  return name;
}  
{% endhighlight %}


### Enum (and not only enum) to single value

{% highlight java %}
public enum TypeEnumWithValue {
    TYPE1(1, "Type A"), TYPE2(2, "Type 2");

    private Integer id;
    private String name;

    @JsonValue
    public String getName() {
        return name;
    }
}
{% endhighlight %}

### Jackson Polymorphic Type Handling Annotations

- `@JsonTypeInfo` – indicates details of what type information to include in serialization
- `@JsonSubTypes` – indicates sub-types of the annotated type
- `@JsonTypeName` – defines a logical type name to use for annotated class

Good example is available on [Baeldung](https://www.baeldung.com/jackson-annotations#jackson-polymorphic-type-handling-annotations)

### Changes in original objects

Skip unknown properties:
`objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);`

<script src="https://gist.github.com/konvi/f8f151a98abc4ad6c3389ba399d294a9.js"></script>
