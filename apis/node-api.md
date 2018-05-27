# [Node API](https://nodejs.org/api/)

@(Node)[node,api]

---

[TOC]

## [Events](https://nodejs.org/api/events.html)

- [Class: EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter)
  - [emitter.emit](https://nodejs.org/api/events.html#events_emitter_emit_eventname_args)
  - [emitter.on](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener)

## [File System](https://nodejs.org/api/fs.html)

- [Class: fs.ReadStream](https://nodejs.org/api/fs.html#fs_class_fs_readstream)
  - [Event: 'open'](https://nodejs.org/api/fs.html#fs_event_open)
- [fs.appendFile](https://nodejs.org/api/fs.html#fs_fs_appendfile_path_data_options_callback)
- [fs.createReadStream](https://nodejs.org/api/fs.html#fs_fs_createreadstream_path_options)
- [fs.open](https://nodejs.org/api/fs.html#fs_fs_open_path_flags_mode_callback)
- [fs.readFile](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback)
- [fs.rename](https://nodejs.org/api/fs.html#fs_fs_rename_oldpath_newpath_callback)
- [fs.unlink](https://nodejs.org/api/fs.html#fs_fs_unlink_path_callback)
- [fs.writeFile](https://nodejs.org/api/fs.html#fs_fs_writefile_file_data_options_callback)

## [HTTP](https://nodejs.org/api/http.html)

- [Class: http.Server](https://nodejs.org/api/http.html#http_class_http_server)
  - [Event: 'request'](https://nodejs.org/api/http.html#http_event_request)
- [Class: http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)
  - [response.end](https://nodejs.org/api/http.html#http_response_end_data_encoding_callback)
  - [response.write](https://nodejs.org/api/http.html#http_response_write_chunk_encoding_callback)
  - [response.writeHead](https://nodejs.org/api/http.html#http_response_writehead_statuscode_statusmessage_headers)

- [Class: http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
  - [message.url](https://nodejs.org/api/http.html#http_message_url)
- [http.createServer](https://nodejs.org/api/http.html#http_http_createserver_options_requestlistener)

## [Modules](https://nodejs.org/api/modules.html)

- [The module scope](https://nodejs.org/api/modules.html#modules_the_module_scope)
  - [require](https://nodejs.org/api/modules.html#modules_require)

## [Net](https://nodejs.org/api/net.html)

- [Class: net.Server](https://nodejs.org/api/net.html#net_class_net_server)
  - [server.listen](https://nodejs.org/api/net.html#net_server_listen)

## [Stream](https://nodejs.org/api/stream.html)

- API for Stream Consumers
  - [Readable Streams](https://nodejs.org/api/stream.html#stream_readable_streams)
    - [Class: stream.Readable](https://nodejs.org/api/stream.html#stream_class_stream_readable)

## [URL](https://nodejs.org/api/url.html)

- The WHATWG URL API
  - [Class: URL](https://nodejs.org/api/url.html#url_class_url)
- Legacy URL API
  - [Legacy urlObject](https://nodejs.org/api/url.html#url_legacy_urlobject)
    - [urlObject.host](https://nodejs.org/api/url.html#url_urlobject_host)
    - [urlObject.pathname](https://nodejs.org/api/url.html#url_urlobject_pathname)
    - [urlObject.query](https://nodejs.org/api/url.html#url_urlobject_query)
    - [urlObject.search](https://nodejs.org/api/url.html#url_urlobject_search)
  - [url.parse](https://nodejs.org/api/url.html#url_url_parse_urlstring_parsequerystring_slashesdenotehost)