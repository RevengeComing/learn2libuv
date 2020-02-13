I will put my experience and explain what i did in the commit

---

At first, On my mac laptop(I am mentioning my platform) I pulled libuv from github and build sharedLibs.

I am following libuv's main user guide:
http://docs.libuv.org/en/v1.x/guide/basics.html

Hello World:
In short, We need to allocate memory for loop `uv_loop_t *loop` and init the memory via `uv_loop_init(loop)` function.
Then running loop with `uv_run(loop, loop's mode)`

And since we dont have an event we close the loop and free the memory.
And i guess uv_run is not blocking method and it will just start the loop and we need to do some blocking ( i guess, i will find it out soon).