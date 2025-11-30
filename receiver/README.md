# General Information

* receiver
  * == 👀how data gets -- into the -- OpenTelemetry Collector👀
    * steps
      * accept data / specified format
      * translates data -- into the -- internal format
      * passes it -- to -- pipelines' 
        * [processors](../processor/README.md)
        * [exporters](../exporter/README.md) 
  * instance's full name
    * == 👀`receiverType/appendedName`👀
      * requirements
        * ⚠️unique⚠️
    * uses
      * | pipelines
  * supported ones
    * core ones
      * [OTLP Receiver](otlpreceiver/README.md)
    * [contrib repository ones](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver)

* goal
  * 👀receiver / available | traces, metrics and logs pipelines👀

## how to configure?

* -- via -- YAML
  * 👀top-level `receivers`👀

* _Example:_ [`exampleReceiver`](example_test.go)

    ```yaml
    # define the receivers
    receivers:
        # Receiver 1.
        # <receiverType>:
        examplereceiver:
            # <setting1>: <value1>
            endpoint: 1.2.3.4:8080
            # ...
        
        # Receiver 2.
        # <receiverType>/<appendedName>:
        examplereceiver/settings:
            # <setting2>: <value2>
            endpoint: 0.0.0.0:9211

    # enable the receivers  == use them
    service:
        pipelines:
            # ALLOWED pipelines: traces, metrics or logs
            # Trace pipeline 1.
            traces:
                receivers: [examplereceiver, examplereceiver/settings]
                processors: []
                exporters: [exampleexporter]
            # Trace pipeline 2.
            traces/another:
                receivers: [examplereceiver, examplereceiver/settings]
                processors: []
                exporters: [exampleexporter]
    ```
  * receiver1's full name == `examplereceiver`
  * receiver2's full name == `examplereceiver/settings`
