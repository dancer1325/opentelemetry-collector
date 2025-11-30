# "otlp" receiver -- `receivers.otlp` --

* goal
  * `receivers.otlp` structure

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
        transport: tcp
        max_recv_msg_size_mib: 100
        max_concurrent_streams: 100
        read_buffer_size: 524288
        write_buffer_size: 524288
        tls:
          ca_file: /path/to/ca.pem
          cert_file: /path/to/cert.pem
          key_file: /path/to/key.pem
          client_ca_file: /path/to/client-ca.pem
        keepalive:
          server_parameters:
            max_connection_idle: 11s
            max_connection_age: 12s
            max_connection_age_grace: 13s
            time: 30s
            timeout: 5s
          enforcement_policy:
            min_time: 10s
            permit_without_stream: true
        auth:
          authenticator: basic_auth
      
      http:
        endpoint: 0.0.0.0:4318
        max_request_body_size: 20971520
        tls:
          ca_file: /path/to/ca.pem
          cert_file: /path/to/cert.pem
          key_file: /path/to/key.pem
          client_ca_file: /path/to/client-ca.pem
        cors:
          allowed_origins:
            - https://*.example.com
            - https://example.com
          allowed_headers:
            - X-Custom-Header
          max_age: 7200
```

## Config -- `receivers.otlp.*` --

| Name      | Type                                              | Default                             | Docs                    |
|-----------|---------------------------------------------------|-------------------------------------|-------------------------|
| protocols | [otlpreceiver-Protocols](#otlpreceiver-protocols) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | == supported protocols  |

### otlpreceiver-Protocols -- `receivers.otlp.protocols.*` --

| Name | Type                                 | Default                             | Docs                                           |
|------|--------------------------------------|-------------------------------------|------------------------------------------------|
| grpc | [configgrpc-GRPCServerSettings](#configgrpc-grpcserversettings) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | == gRPC server configuration's common settings |
| http | [confighttp-HTTPServerSettings](#confighttp-httpserversettings) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | == HTTP server settings                        |

#### configgrpc-GRPCServerSettings -- `receivers.otlp.protocols.grpc.*` --

| Name                   | Type                                                                  | Default      | Docs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|------------------------|-----------------------------------------------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| endpoint               | string                                                                | localhost:4317 | Endpoint configures the address for this network connection. For TCP and UDP networks, the address has the form "host:port". The host must be a literal IP address, or a host name that can be resolved to IP addresses. The port must be a literal port number or a service name. If the host is a literal IPv6 address it must be enclosed in square brackets, as in "[2001:db8::1]:80" or "[fe80::1%zone]:80". The zone specifies the scope of the literal IPv6 address as defined in RFC 4007. |
| transport              | string                                                                | tcp          | Transport to use. Known protocols are "tcp", "tcp4" (IPv4-only), "tcp6" (IPv6-only), "udp", "udp4" (IPv4-only), "udp6" (IPv6-only), "ip", "ip4" (IPv4-only), "ip6" (IPv6-only), "unix", "unixgram" and "unixpacket".                                                                                                                                                                                                                                                                               |
| tls                    | [configtls-TLSServerSetting](#configtls-tlsserversetting)             | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | Configures the protocol to use TLS. The default value is nil, which will cause the protocol to not use TLS.                                                                                                                                                                                                                                                                                                                                                                                        |
| max_recv_msg_size_mib  | uint64                                                                | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | MaxRecvMsgSizeMiB sets the maximum size (in MiB) of messages accepted by the server.                                                                                                                                                                                                                                                                                                                                                                                                               |
| max_concurrent_streams | uint32                                                                | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | MaxConcurrentStreams sets the limit on the number of concurrent streams to each ServerTransport. It has effect only for streaming RPCs.                                                                                                                                                                                                                                                                                                                                                            |
| read_buffer_size       | int                                                                   | 524288       | ReadBufferSize for gRPC server. See grpc.ReadBufferSize (https://godoc.org/google.golang.org/grpc#ReadBufferSize).                                                                                                                                                                                                                                                                                                                                                                                 |
| write_buffer_size      | int                                                                   | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | WriteBufferSize for gRPC server. See grpc.WriteBufferSize (https://godoc.org/google.golang.org/grpc#WriteBufferSize).                                                                                                                                                                                                                                                                                                                                                                              |
| keepalive              | [configgrpc-KeepaliveServerConfig](#configgrpc-keepaliveserverconfig) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | Keepalive anchor for all the settings related to keepalive.                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| auth                   | [configauth-Authentication](#configauth-authentication)               | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | Auth for this receiver                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

##### configtls-TLSServerSetting -- `receivers.otlp.protocols.grpc.tls.*` --

| Name           | Type   | Default    | Docs                                                                                                                                                                                                                                                             |
|----------------|--------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ca_file        | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the CA cert. For a client this verifies the server certificate. For a server this verifies client certificates. If empty uses system root CA. (optional)                                                                                                 |
| cert_file      | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS cert to use for TLS required connections. (optional)                                                                                                                                                                                             |
| key_file       | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS key to use for TLS required connections. (optional)                                                                                                                                                                                              |
| client_ca_file | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS cert to use by the server to verify a client certificate. (optional) This sets the ClientCAs and ClientAuth to RequireAndVerifyClientCert in the TLSConfig. Please refer to https://godoc.org/crypto/tls#Config for more information. (optional) |

##### configgrpc-Keepalive -- `receivers.otlp.protocols.grpc.keepalive.*` --

| Name               | Type                                                                            | Default    | Docs                                                                                                                                                                                                                                                                          |
|--------------------|---------------------------------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server_parameters  | [configgrpc-KeepaliveServerParameters](#configgrpc-keepaliveserverparameters)   | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | KeepaliveServerParameters allow configuration of the keepalive.ServerParameters. The same default values as keepalive.ServerParameters are applicable and get applied by the server. See https://godoc.org/google.golang.org/grpc/keepalive#ServerParameters for details.     |
| enforcement_policy | [configgrpc-KeepaliveEnforcementPolicy](#configgrpc-keepaliveenforcementpolicy) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | KeepaliveEnforcementPolicy allow configuration of the keepalive.EnforcementPolicy. The same default values as keepalive.EnforcementPolicy are applicable and get applied by the server. See https://godoc.org/google.golang.org/grpc/keepalive#EnforcementPolicy for details. |

###### configgrpc-KeepaliveServerParameters -- `receivers.otlp.protocols.grpc.keepalive.server_parameters.*` --

| Name                     | Type                            | Default    | Docs |
|--------------------------|---------------------------------|------------|------|
| max_connection_idle      | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |
| max_connection_age       | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |
| max_connection_age_grace | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |
| time                     | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |
| timeout                  | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |

###### configgrpc-KeepaliveEnforcementPolicy -- `receivers.otlp.protocols.grpc.keepalive.enforcement_policy.*` --

| Name                  | Type                            | Default    | Docs |
|-----------------------|---------------------------------|------------|------|
| min_time              | [time-Duration](#time-duration) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |
| permit_without_stream | bool                            | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL |      |

##### configauth-Authentication -- `receivers.otlp.protocols.grpc.auth.*` --

| Name          | Type   | Default    | Docs                                                                                                           |
|---------------|--------|------------|----------------------------------------------------------------------------------------------------------------|
| authenticator | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | AuthenticatorName specifies the name of the extension to use in order to authenticate the incoming data point. |

### confighttp-HTTPServerSettings -- `receivers.otlp.protocols.http.*` --

| Name                  | Type                                                      | Default      | Docs                                                                                                                                    |
|-----------------------|-----------------------------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| endpoint              | string                                                    | localhost:4318 | Endpoint configures the listening address for the server.                                                                               |
| tls                   | [configtls-TLSServerSetting](#configtls-tlsserversetting) | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | TLSSetting struct exposes TLS client configuration.                                                                                     |
| cors                  | [confighttp-CORSConfig](#confighttp-corsconfig)           | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL   | CORSConfig configures a receiver for HTTP cross-origin resource sharing (CORS).                                                       |
| max_request_body_size | int                                                       | 20971520     | MaxRequestBodySize configures the maximum allowed body size in bytes for a single request. The default `20971520` means 20MiB           |

#### confighttp-CORSConfig -- `receivers.otlp.protocols.http.cors.*` --

| Name            | Type     | Default    | Docs                                                                                                                                                                                                                                                                                       |
|-----------------|----------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| allowed_origins | []string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | AllowedOrigins sets the allowed values of the Origin header for HTTP/JSON requests to an OTLP receiver. An origin may contain a wildcard (`*`) to replace 0 or more characters (e.g., `"https://*.example.com"`, or `"*"` to allow any origin).                                            |
| allowed_headers | []string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | AllowedHeaders sets what headers will be allowed in CORS requests. The Accept, Accept-Language, Content-Type, and Content-Language headers are implicitly allowed. If no headers are listed, X-Requested-With will also be accepted by default. Include `"*"` to allow any request header. |
| max_age         | int      | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | MaxAge sets the value of the Access-Control-Max-Age response header.  Set it to the number of seconds that browsers should cache a CORS preflight response for.                                                                                                                            |

#### configtls-TLSServerSetting -- `receivers.otlp.protocols.http.tls.*` --

| Name           | Type   | Default    | Docs                                                                                                                                                                                                                                                             |
|----------------|--------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ca_file        | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the CA cert. For a client this verifies the server certificate <br/> For a server this verifies client certificates. If empty uses system root CA. (optional)                                                                                            |
| cert_file      | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS cert to use for TLS required connections. (optional)                                                                                                                                                                                             |
| key_file       | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS key to use for TLS required connections. (optional)                                                                                                                                                                                              |
| client_ca_file | string | ❌NO❌ <br/> &nbsp;&nbsp; == OPTIONAL | Path to the TLS cert to use by the server to verify a client certificate. (optional) This sets the ClientCAs and ClientAuth to RequireAndVerifyClientCert in the TLSConfig. Please refer to https://godoc.org/crypto/tls#Config for more information. (optional) |

### time-Duration 
* == OPTIONAL signed sequence of decimal numbers + unit suffix
  * OPTIONAL sign
  * _Examples:_ `300ms`, `-1.5h`, or `2h45m`
* ALLOWED time units
  * `ns`,
  * `us`,
  * `ms`,
  * `s`,
  * `m`,
  * `h`
