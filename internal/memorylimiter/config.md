* TODO:
var (
	errCheckIntervalOutOfRange        = errors.New("'check_interval' must be greater than zero")
	errInconsistentGCMinInterval      = errors.New("'min_gc_interval_when_soft_limited' should be larger than 'min_gc_interval_when_hard_limited'")
	errLimitOutOfRange                = errors.New("'limit_mib' or 'limit_percentage' must be greater than zero")
	errSpikeLimitOutOfRange           = errors.New("'spike_limit_mib' must be smaller than 'limit_mib'")
	errSpikeLimitPercentageOutOfRange = errors.New("'spike_limit_percentage' must be smaller than 'limit_percentage'")
	errLimitPercentageOutOfRange      = errors.New(
		"'limit_percentage' and 'spike_limit_percentage' must be greater than zero and less than or equal to hundred")
)

* `type Config struct {}`
  * == 👀memory memoryLimiter processor configuration👀 
  * `check_interval`
    * == 👀time BETWEEN measurements of memory usage👀
      * _Example:_ measureOfMemoryUsae | t1
    * by default,
      * 0s
        * == ❌NO perform checks❌
    * recommended value
      * 1 second
    * uses
      * avoid exceeding the memory usage limits
    * ⚠️MANDATORY to specify it⚠️
    * If the expected traffic to the Collector is very spiky then decrease the `check_interval`
	  or increase `spike_limit_mib` to avoid memory usage going over the hard limit.
  * `min_gc_interval_when_soft_limited`
    * MinGCIntervalWhenSoftLimited minimum interval between forced GC when in soft (=limit_mib - spike_limit_mib) limited mode
    * Zero value means no minimum interval
    * GCs is a CPU-heavy operation and executing it too frequently may affect the recovery capabilities of the collector
    * 👀OPTIONAL👀
  * `min_gc_interval_when_hard_limited`
    * MinGCIntervalWhenHardLimited minimum interval between forced GC when in hard (=limit_mib) limited mode
    * Zero value means no minimum interval
    * GCs is a CPU-heavy operation and executing it too frequently may affect the recovery capabilities of the collector
    * 👀OPTIONAL👀
  * `limit_mib`
    * MemoryLimitMiB is the maximum amount of memory, in MiB, targeted to be
              // allocated by the process
    * ⚠️MANDATORY to specify it⚠️
  * `spike_limit_mib`
    * == maximum, in MiB, spike expected between the measurements of memory usage
    * ⚠️MANDATORY to specify it⚠️
  * `limit_percentage`
    * MemoryLimitPercentage is the maximum amount of memory, in %, targeted to be allocated by the process
    * The fixed memory settings MemoryLimitMiB has a higher precedence.
    * ⚠️MANDATORY to specify it⚠️
  * `spike_limit_percentage`
    * == maximum, in percents against the total memory, spike expected between the measurements of memory usage
    * ⚠️MANDATORY to specify it⚠️
