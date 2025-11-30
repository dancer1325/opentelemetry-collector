* `type Config struct {}`
  * == batch processor's configuration 
  * `timeout`
    * == time / AFTER it,
      * batch will be sent 
        * ⚠️regardless of size⚠️
    * by default,
      * 200 ms
    * if it's set 0 -> as soon as data comes -> batched data will be sent IMMEDIATELY
      * == ignore `send_batch_size`
        * ⚠️ALTHOUGH batched data's size <= `send_batch_size`⚠️ 
  * `send_batch_size`
    * by default,
      * 8192
    * TODO: SendBatchSize is the size of a batch which after hit, will trigger it to be sent
    * When this is set to zero, the batch size is ignored and data will be sent immediately subject to only send_batch_max_size
  * `send_batch_max_size`
    * SendBatchMaxSize is the maximum size of a batch
    * It must be larger than SendBatchSize
    * Larger batches are split into smaller units
    * Default value is 0, that means no maximum size.

        // MetadataKeys is a list of client.Metadata keys that will be
        // used to form distinct batchers.  If this setting is empty,
        // a single batcher instance will be used.  When this setting
        // is not empty, one batcher will be used per distinct
        // combination of values for the listed metadata keys.
        //
        // Empty value and unset metadata are treated as distinct cases.
        //
        // Entries are case-insensitive.  Duplicated entries will
        // trigger a validation error.
        MetadataKeys []string `mapstructure:"metadata_keys"`

        // MetadataCardinalityLimit indicates the maximum number of
        // batcher instances that will be created through a distinct
        // combination of MetadataKeys.
        MetadataCardinalityLimit uint32 `mapstructure:"metadata_cardinality_limit"`
        // prevent unkeyed literal initialization
        _ struct{}
    }
