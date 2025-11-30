// supportedLevels in this exporter's configuration.
// configtelemetry.LevelNone and other future values are not supported.
var supportedLevels map[configtelemetry.Level]struct{} = map[configtelemetry.Level]struct{}{
	configtelemetry.LevelBasic:    {},
	configtelemetry.LevelNormal:   {},
	configtelemetry.LevelDetailed: {},
}
 
* `type Config struct {}`
  * debug exporter's configuration
  * `verbosity,omitempty`
    * == debug exporter verbosity
    * by default,
      * `basic`
    * OPTIONAL
  * `sampling_initial`
    * OPTIONAL
    * defines how many samples are initially logged during each second.
  * `sampling_thereafter`
    * OPTIONAL
    * defines the sampling rate after the initial samples are logged.
  * `use_internal_logger`
    * defines whether the exporter sends the output to the collector's internal logger
    * OPTIONAL
  * `sending_queue`
    * OPTIONAL
