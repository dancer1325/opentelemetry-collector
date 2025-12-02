# General Information

* exporter
  * == 👀how the pipeline data leaves the collector👀
  * AVAILABLE | ALL telemetry data (traces, metrics and logs)
  * supported
    * core ones
      - [Debug](debugexporter/README.md)
      - [OTLP gRPC](otlpexporter/README.md)
      - [OTLP HTTP](otlphttpexporter/README.md)
    * [contrib ones](https://github.com/open-telemetry/opentelemetry-collector-contrib)

## Configuring Exporters

* configuration -- via -- YAML
  * top-level `exporters`
 
* exporter full names
  * == exporterType + '/' + customName 
  * uses
    * refer | pipelines
  * MUST be UNIQUE

## Data Ownership

* TODO: When multiple exporters are configured to send the same data (e.g. by configuring multiple
exporters for the same pipeline):
* exporters *not* configured to mutate the data will have shared access to the data
* exporters with the Capabilities to mutate the data will receive a copy of the data

Exporters access export data when `ConsumeTraces`/`ConsumeMetrics`/`ConsumeLogs` function is called
* Unless exporter's capabilities include mutation, the exporter MUST NOT modify the `pdata.Traces`/`pdata.Metrics`/`pdata.Logs` argument of
these functions
* Any approach that does not mutate the original `pdata.Traces`/`pdata.Metrics`/`pdata.Logs` is allowed without the mutation capability.

## Proxy Support

Beyond standard YAML configuration as outlined in the individual READMEs above,
exporters that leverage the net/http package (all do today) also respect the
following proxy environment variables:

- HTTP_PROXY
- HTTPS_PROXY
- NO_PROXY

If set at Collector start time then exporters, regardless of protocol,
will or will not proxy traffic as defined by these environment variables.
