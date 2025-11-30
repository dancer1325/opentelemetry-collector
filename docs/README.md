# OpenTelemetry Collector

* **Status**
  * [Beta](https://github.com/open-telemetry/opentelemetry-specification/blob/main/oteps/0232-maturity-of-otel.md#beta)

* OpenTelemetry Collector
  * == 💡2 mechanismS💡
    * mechanism / 
      * load & parse an [OpenTelemetry Collector configuration file](#configuration-file)
    * mechanism / 
      * can include compatible [Collector components](#opentelemetry-collector-components) /
        * user wishes to include
  * allows
    * easily switch BETWEEN [OpenTelemetry Collector Distributions](#opentelemetry-collector-distribution-distro)
      * Reason:🧠thanks to BOTH mechanismS🧠
    * OpenTelemetry Collector's produced components can work | any OpenTelemetry Collector supported vendor 

## Configuration file

* == YAML 
  * 👀[minimum structure](../otelcol/config.go)👀

    ```yaml
    receivers:
    processors:
    exporters:
    connectors:
    extensions:
    service:
      telemetry:
      pipelines:
    ```
    * [`receivers`](../receiver/README.md)
    * [`processors`](../processor/README.md)
    * [`exporters`](../exporter/README.md)
    * TODO:

## OpenTelemetry Collector components

* OpenTelemetry Collector component
  * ⚠️requirements⚠️
    * implement a [`component` interface](../component/component.go)
    * [unique `ID`](../component/identifiable.go)
      * uses
        * MULTIPLE componentS / SAME name BUT DIFFERENT IDs -> can be used SIMULTANEOUSLY | 1! OpenTelemetry Collector
  * uses
    * library / want to be an OpenTelemetry Collector component

### Compatibility requirements

* Collector component is compatible -- with an -- OpenTelemetry Collector  
  * ⚠️requirements⚠️
    * ALL its dependencies are -- with that -- Collector's Component interfaces
      * source-compatible &
      * version-compatible  
  * _Example:_ Collector derived -- from -- [OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector) v0.100.0
    * _MUST_ support 
      * ALL components / version-compatible -- with -- `github.com/open-telemetry/opentelemetry-collector/component` v0.100.0

## OpenTelemetry Collector Distribution (Distro)

* == OpenTelemetry Collector's compiled instance / ⚠️specific set of components & features⚠️
  * if you include NEW components -> DIFFERENT Distribution

* Distribution author 
  * _MAY_ choose to produce a 
    * distribution -- by -- utilizing tools, &/OR
    * documentation / supported by the OpenTelemetry project
  * _MUST_ provide
    * end users / 
      * can add their OWN components | Distribution's components
