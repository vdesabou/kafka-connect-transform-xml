# Introduction

This project provides transformations for Kafka Connect that will convert XML text to a Kafka Connect
struct based on the configured XML schema. This transformation works by dynamically generating 
JAXB source with [XJC](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/xjc.html) with the
[xjc-kafka-connect-plugin](https://github.com/jcustenborder/xjc-kafka-connect-plugin) loaded. This 
allows the transformation to efficiently convert XML to structured data for Kafka connect.

Use it in conjunction with a Source connector that reads XML data, such as from a [HTTP REST endpoint](https://www.confluent.io/hub/castorm/kafka-connect-http).

# Transformations

## FromXML(Key)

This transformation is used to transform XML in the Value of the input into a JSON struct based on the provided XSD. 

### Configuration

| Name         | Type   | Importance | Default Value | Validator | Documentation                                                   |
| ------------ | ------ | ---------- | ------------- | --------- | --------------------------------------------------------------- |
| schema.path  | List   | High       |               |           | Urls to the schemas to load. http and https paths are supported |
| xjc.options.automatic.name.conflict.resolution.enabled| Boolean | | False |||
| xjc.options.strict.check.enabled | Boolean | | True |||
| xjc.options.verbose.enabled | Boolean | | False |||

#### Standalone Example

```properties
transforms=xml_key
transforms.xml_key.type=com.github.jcustenborder.kafka.connect.transform.xml.FromXml$Key
# The following values must be configured.
transforms.xml_key.schema.path = http://web.address/my.xsd
```

#### Distributed Example

```json
"transforms": "xml_key",
"transforms.xml_key.type": "com.github.jcustenborder.kafka.connect.transform.xml.FromXml$Key",
"transforms.xml_key.schema.path": "http://web.address/my.xsd"
```

## FromXML(Value)

This transformation is used to transform XML in the Value of the input into a JSON struct based on the provided XSD. 

### Configuration

| Name         | Type   | Importance | Default Value | Validator | Documentation                                                   |
| ------------ | ------ | ---------- | ------------- | --------- | --------------------------------------------------------------- |
| schema.path  | List   | High       |               |           | Urls to the schemas to load. http and https paths are supported |
| xjc.options.automatic.name.conflict.resolution.enabled| Boolean | | False |||
| xjc.options.strict.check.enabled | Boolean | | True |||
| xjc.options.verbose.enabled | Boolean | | False |||


#### Standalone Example

```properties
transforms=xml_value
transforms.xml_value.type=com.github.jcustenborder.kafka.connect.transform.xml.FromXml$Value
# The following values must be configured.
transforms.xml_value.schema.path = http://web.address/my.xsd
```

#### Distributed Example

```json
"transforms": "xml_value",
"transforms.xml_value.type": "com.github.jcustenborder.kafka.connect.transform.xml.FromXml$Value",
"transforms.xml_value.schema.path": "< Configure me >"
```


#### Build with Docker

```
docker run -i --rm -v "${PWD}/kafka-connect-transform-xml":/usr/src/mymaven -v "/Users/vsaboulin/Documents/github/kafka-docker-playground/scripts/settings.xml:/tmp/settings.xml" -v "$HOME/.m2":/root/.m2  -v "${PWD}/kafka-connect-transform-xml/target:/usr/src/mymaven/target" -w /usr/src/mymaven maven:3.6.1-jdk-11 mvn -s /tmp/settings.xml install -DskipTests -Dmaven.javadoc.skip=true
```