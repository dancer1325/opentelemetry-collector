* `type Config struct {}`
  * == batch processor's configuration 
  * `timeout`
    * == time / AFTER it,
      * batch will be sent 
        * ⚠️regardless of `send_batch_size`⚠️
    * by default,
      * 200 ms
    * if it's set 0 -> as soon as data comes -> batched data will be sent IMMEDIATELY
      * == ⚠️ignore `send_batch_size`⚠️
        * ⚠️ALTHOUGH batched data's size <= `send_batch_size`⚠️ 
  * `send_batch_size`
    * == number of (spans OR metric data points OR log records) / 👀afterward, a batch is sent👀
      * regardless of the `timeout`
      * == trigger
        * == ❌NOT affect batch's size❌
          * see `send_batch_max_size`
    * by default,
      * 8192
    * if you set 0 ->
      * batch size is ignored
      * data is sent IMMEDIATELY
        * batch's size restriction: < `send_batch_max_size`
  * `send_batch_max_size`
    * == batch size maximum
      * by default, 
        * 0   == NO maximum size
      * as larger -> split | smaller units 
    * requirements
      * \>= `send_batch_size`
  * `metadata_keys`
    * == client's metadata keys /
      * uses
        * 👀form DISTINCT batchers👀
    * == `[]string`
      * case-insensitive
      * duplicated entries -> validation error
      * empty != unset
      * by default,
        * empty
    * if it's
      * empty -> use 1! batcher instance
      * 👀NOT empty -> 1 batcher instance / DIFFERENT combination of `client.Metadata` values👀
        * -> increase memory / dedicated to batching
  * `metadata_cardinality_limit`
    * == maximum number of batcher instances / 
      * created -- through a -- DIFFERENT combination of MetadataKeys
      * by default,
        * 1000
    * if it's NOT empty -> restricts the number of UNIQUE combinations of `metadata_keys`' values / processed | lifetime of the process

* batcher instance
  * == goroutine /
    * collector creates
    * manages the batching of specific metadata
    * 's lifetime == OpenTelemetry Collector's lifetime