Ansible Role: nginx
=========

DEPRECATION NOTICE: nginx has similar modules, so this is being deprecated. 

An Ansible Role that install and configure nginx on RHEL/CentOS, Fedora and Debian/Ubuntu. The configuration is focused on security.

Requirements
------------

No requirements.

Role Variables
--------------

**Available variables are listed below, along with default values (see defaults/main.yml):**

nginx_level2_strict_check: false

If set to true it will abort the execution with error if a level 2 issue is found. Level 2 has the following characteristics:
  - are intended for environments or use cases where security is paramount
  - acts as defense in depth measure
  - may negatively inhibit the utility or performance of the technology

.

    #nginx_worker_processes: "{{ ansible_facts['ansible_processor_vcpus'] | default('auto'}}"

Defines the number of worker processes. The value “auto” will try to autodetect the number of CPU.


    #nginx_worker_rlimit_nofile: 15000

Changes the limit on the maximum number of open files (RLIMIT_NOFILE) for worker processes. Used to increase the limit without restarting the main process.

    nginx_multi_accept: false

multi_accept false: a worker accepts just a single connection, handles it and then returns to the kernel for the next event to process.

    nginx_use_epoll: false

Efficient method for processing connections (requires Linux 2.6+)

    nginx_worker_connections: 2048

Sets the maximum number of simultaneous connections that can be opened by a
worker process.
 - max clients = worker_connections * worker_processes
 - max clients is also limited by the number of socket connections available on the system (~64k)

.

    #nginx_server_names_hash_bucket_size: 64

Sets the bucket size for the server names hash tables. The default value depends on the size of the processor’s cache line.


    nginx_types_hash_max_size: 4096

    nginx_keepalive_timeout: "5 5"

The first parameter assigns the timeout for keep-alive connections with the client. The server will close connections after this time. The optional second parameter assigns the time value in the header Keep-Alive: timeout=time of the response. This header can convince some browsers to close the connection, so that the server does not have to. Without this parameter, nginx does not send a Keep-Alive header (though this is not what makes a connection “keep-alive”).


    nginx_send_timeout: 10

Directive assigns response timeout to client. Timeout is established not on entire transfer of answer, but only between two operations of reading, if after this time client will take nothing, then nginx is shutting down the connection.

    nginx_ssl_protocols: "TLSv1.2 TLSv1.3"

Set ssl protocols. Not recommended to add older protocols due to security concerns.

> The TLSv1.1 and TLSv1.2 parameters (1.1.13, 1.0.12) work only when OpenSSL 1.0.1 or higher is used.
> The TLSv1.3 parameter (1.13.0) works only when OpenSSL 1.1.1 or higher is used.

    nginx_ssl_session_cache: "shared:SSL:50m"

Speed up server by caching ssl session information. See [documentation](https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache) for more information.

    nginx_ssl_session_timeout: "5m"

Specifies a time during which a client may reuse the session parameters.

    nginx_proxy_ssl_protocols: "TLSv1.2 TLSv1.3"

Enables the specified protocols for requests to a proxied HTTPS server.

> The TLSv1.1 and TLSv1.2 parameters (1.1.13, 1.0.12) work only when OpenSSL 1.0.1 or higher is used.
> The TLSv1.3 parameter (1.13.0) works only when OpenSSL 1.1.1 or higher is used.

    nginx_ssl_prefer_server_ciphers: false

Specifies that server ciphers should be preferred over client ciphers when using the SSLv3 and TLS protocols.

    nginx_ssl_ciphers:
      - ECDHE-ECDSA-AES128-GCM-SHA256
      - ECDHE-RSA-AES128-GCM-SHA256
      - ECDHE-ECDSA-AES256-GCM-SHA384
      - ECDHE-RSA-AES256-GCM-SHA384
      - ECDHE-ECDSA-CHACHA20-POLY1305
      - ECDHE-RSA-CHACHA20-POLY1305
      - DHE-RSA-AES128-GCM-SHA256
      - DHE-RSA-AES256-GCM-SHA384

Specifies the enabled ciphers. The ciphers are specified in the format understood by the OpenSSL library. The full list can be viewed using the “openssl ciphers” command.

    nginx_client_body_timeout: 10

Directive sets the read timeout for the request body from client. The timeout is set only if a body is not get in one readstep. If after this time the client send nothing, nginx returns error “Request time out” (408).

    nginx_client_header_timeout: 10

Directive assigns timeout with reading of the title of the request of client.
The timeout is set only if a header is not get in one readstep. If after this time the client send nothing, nginx returns error “Request time out” (408).

    nginx_client_body_buffer_size: "1K"

The directive specifies the client request body buffer size.

    nginx_client_header_buffer_size: "1k"

Directive sets the headerbuffer size for the request header from client. For the overwhelming majority of requests a buffer size of 1K is sufficient. Increase this if you have a custom header or a large cookie sent from the client (e.g., wap client).

    nginx_client_max_body_size: "100K"

Directive assigns the maximum accepted body size of client request, indicated by the line Content-Length in the header of request. If size is greater the given one, then the client gets the error “Request Entity Too Large” (413). Increase this when you are getting file uploads via the POST method.

    nginx_large_client_header_buffers: "2 1k"

Directive assigns the maximum number and size of buffers for large headers to read from client request. By default the size of one buffer is equal to the size of page, depending on platform this either 4K or 8K, if at the end of working request connection converts to state keep-alive, then these buffers are freed. 2x1k will accept 2kB data URI. This will also help combat bad bots and DoS attacks.

    nginx_slimit_period: "5m"

1m can handle 32000 sessions with 32 bytes/session

    nginx_slimit_connections: 5

Number of simultaneous connections for same ip address

    nginx_gzip_vary: false

Enables or disables inserting the "Vary: Accept-Encoding" response header field if the directives gzip, gzip_static, or gunzip are active.

    nginx_gzip_proxied: "off"

Enables or disables gzipping of responses for proxied requests depending on the request and response. The fact that the request is proxied is determined by the presence of the "Via" request header field. Valid values: off, expired, no-cache, no-store, private, no_last_modified, no_etag, auth, any

    #nginx_gzip_comp_level: 5

Sets a gzip compression level of a response. Acceptable values are in the range from 1 to 9.

    #nginx_gzip_buffers: "16 8k"

Sets the number and size of buffers used to compress a response. By default, the buffer size is equal to one memory page. This is either 4K or 8K, depending on a platform.

    #nginx_gzip_http_version: "1.1"

Sets the minimum HTTP version of a request required to compress a response.

    nginx_gzip_types:
      - text/plain
      - text/css
      - application/json
      - application/javascript
      - text/xml
      - application/xml
      - application/xml+rss
      - text/javascript
      - image/svg+xml
      - application/xhtml+xml
      - application/atom+xml

Enables gzipping of responses for the specified MIME types in addition to "text/html". The special value "*" matches any MIME type (0.8.29). Responses with the "text/html" type are always compressed.

    #nginx_default_ssl_certificate: "/etc/letsencrypt/live/example.net/fullchain.pem"
    #nginx_default_ssl_certificate_key: "/etc/letsencrypt/live/example.net/privkey.pem"

If defined, set a default certificate to be used on the virtual sites. This is more efficient when using the same certificate for several virtual sites.

    nginx_allow_access_domains: []

If bot is just making random server scan for all domains, just deny it. You must only allow configured virtual domain or reverse proxy requests. You don’t want to display request using an IP address

    nginx_allowed_methods:
      - GET
      - HEAD
      - POST

GET and POST are the most common methods on the Internet. Web server methods are defined in RFC 2616. If a web server does not require the implementation of all available methods, they should be disabled.

    nginx_proxy_sites: []
    #  - name: pihole
    #    server_name: pihole.example.com
    #    access_log_name: pihole_access.log
    #    error_log_name: pihole_error.log
    #    proxy_pass: http://localhost:9090/
    #    ssl_certificate: /etc/letsencrypt/live/example.com/fullchain.pem
    #    ssl_certificate_key: /etc/letsencrypt/live/example.com/privkey.pem
    #    using_certbot: false

List of proxy configuration to set

    nginx_site_files: []

List of fiiles to copy to conf.d directory. Create files for virtual sites then add the names in this list in order to have them copied.

    nginx_create_redhat_default_site: false

RedHat based distros only: create default site configuration

    nginx_open_file_cache: "max=1500 inactive=30s"

Configures a cache that can store: open file descriptors, their sizes and modification times; information on existence of directories; file lookup errors, such as "file not found", "no read permission", and so on

    nginx_open_file_cache_valid: "30s"

Sets a time after which open_file_cache elements should be validated.

    nginx_open_file_cache_min_uses: 5

Sets the minimum number of file accesses during the period configured by the inactive parameter of the open_file_cache directive, required for a file descriptor to remain open in the cache.

    nginx_open_file_cache_errors: "off"

Enables or disables caching of file lookup errors by open_file_cache.

**The variables listed below do not need to be changed for targeted systems (see vars/main.yml):**

    nginx_username:

Username that runs nginx processes. The value changes depending on the operating system.

    nginx_packages:

Package list to install nginx server.

    nginx_modules_location:

Module location.

Dependencies
------------

No dependencies.

Example Playbook
----------------

    - hosts: servers
      vars:
        nginx_proxy_sites:
          - name: testsite
            server_name: testsite.example.com
            access_log_name: testsite_access.log
            error_log_name: testsite_error.log
            proxy_pass: http://localhost:9090/
            using_certbot: false
        nginx_create_redhat_default_site: true
      roles:
         - { role: guidugli.nginx }

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2020 by Carlos Guidugli.
