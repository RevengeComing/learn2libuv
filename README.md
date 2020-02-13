Filesystem:

Simple filesystem read/write is achieved using the uv_fs_* functions and the uv_fs_t struct.

> Note
The libuv filesystem operations are different from socket operations. Socket operations use the non-blocking operations provided by the operating system. Filesystem operations use blocking functions internally, but invoke these functions in a thread pool and notify watchers registered with the event loop when application interaction is required. 

All filesystem functions have two forms - synchronous and asynchronous.

The synchronous forms automatically get called (and block) if the callback is null. The return value of functions is a libuv error code. This is usually only useful for synchronous calls. The asynchronous form is called when a callback is passed and the return value is 0.

----

There's some functions like 

`uv_fs_open` To open a file. req->result of `uv_fs_open` is a file descriptor.
`uv_fs_read` To read an opened file. req->result of `uv_fs_read` is amount of readed bytes, if there's an error it is negative number.
`uv_fs_write` To write into shell or write into opened file. req->result of `uv_fs_write` is negative if there's an error. `uv_fs_write` with uv_file = 1 will write into terminal.