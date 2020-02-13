SOME NOTES:
`uv_default_loop()` returns default loop which initilized first.

Initialization functions or synchronous functions which may fail return a negative number on error. Async functions that may fail will pass a status parameter to their callbacks. The error messages are defined as UV_E* constants. You can use the uv_strerror(int) and uv_err_name(int) functions to get a const char * describing the error or the error name respectively.

I/O read callbacks (such as for files and sockets) are passed a parameter nread. If nread is less than 0, there was an error (UV_EOF is the end of file error, which you may want to handle differently).

---

libuv works by the user expressing interest in particular events. This is usually done by creating a handle to an I/O device, timer or process.
Handles are opaque structs named as uv_TYPE_t where type signifies what the handle is used for.

```
/* Handle types. */
typedef struct uv_loop_s uv_loop_t;
typedef struct uv_handle_s uv_handle_t;
typedef struct uv_dir_s uv_dir_t;
typedef struct uv_stream_s uv_stream_t;
typedef struct uv_tcp_s uv_tcp_t;
typedef struct uv_udp_s uv_udp_t;
typedef struct uv_pipe_s uv_pipe_t;
typedef struct uv_tty_s uv_tty_t;
typedef struct uv_poll_s uv_poll_t;
typedef struct uv_timer_s uv_timer_t;
typedef struct uv_prepare_s uv_prepare_t;
typedef struct uv_check_s uv_check_t;
typedef struct uv_idle_s uv_idle_t;
typedef struct uv_async_s uv_async_t;
typedef struct uv_process_s uv_process_t;
typedef struct uv_fs_event_s uv_fs_event_t;
typedef struct uv_fs_poll_s uv_fs_poll_t;
typedef struct uv_signal_s uv_signal_t;

/* Request types. */
typedef struct uv_req_s uv_req_t;
typedef struct uv_getaddrinfo_s uv_getaddrinfo_t;
typedef struct uv_getnameinfo_s uv_getnameinfo_t;
typedef struct uv_shutdown_s uv_shutdown_t;
typedef struct uv_write_s uv_write_t;
typedef struct uv_connect_s uv_connect_t;
typedef struct uv_udp_send_s uv_udp_send_t;
typedef struct uv_fs_s uv_fs_t;
typedef struct uv_work_s uv_work_t;
```

If we want to create a handle do `uv_TYPE_init(uv_loop_t *, uv_TYPE_t *)` for example for a udp socket we are going to write `uv_udp_init(uv_loop_t *, uv_TYPE_t *)`.

---

Idler pattern:
The callbacks of idle handles are invoked once per event loop. The idle callback can be used to perform some very low priority activity. For example, you could dispatch a summary of the daily application performance to the developers for analysis during periods of idleness, or use the application’s CPU time to perform SETI calculations :) An idle watcher is also useful in a GUI application. Say you are using an event loop for a file download. If the TCP socket is still being established and no other events are present your event loop will pause (block), which means your progress bar will freeze and the user will face an unresponsive application. In such a case queue up and idle watcher to keep the UI operational.

----

Now we are creating an idle event.
`uv_idle_t` type is a handle for idle events, initilize an idle handle with `uv_idle_init(uv_default_loop(), &idler)`, and  register `wait_for_a_while(uv_idle_t* handle)` event and start idle handle with `uv_idle_start(&idler, wait_for_a_while)`.

The `wait_for_a_while` happens once per event loop and since we have not registered event in our event loop it, the `wait_for_a_while` event happens 10e6 times and the program stops.