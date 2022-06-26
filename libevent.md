# create event 
API
- event_assign // init context of struct event (**NOT Recommand**, this will dependent size of struct event, Poor Compatibility)
- event_new // allocate memory, and init context (call event_assign)
- event_add // add to evbase
- event_del // remove from evbase
- event_free // event_del, and free memory

1. create timer (one-shot, persist)
```c
// @remark event_new, must use event_del to release resource.
//         in this demo, one-shot event has memory leak. (just for simple)
#include <stdio.h>
#include <event2/event.h>

void timer_cb(evutil_socket_t sock, short event, void *priv)
{
  struct event *timer = *(struct event **)priv;
  static int cnt = 0;
  
  printf("timeout call, cnt=%d\n", cnt++);
 
  if (cnt > 3) {
      event_free(timer); // event_free also do event_del in it
  }
}

int main(void)
{
 struct event_base *base = event_base_new();
 struct timeval tv = {1, 0}; // one second
 struct event *timer = NULL;
 
 short event = 0;             // one-shot timer created.
 // short event = EV_PERSIST; // persist timer
 
 timer = event_new(base, -1, event, timer_cb, &timer);
 event_add(timer, &tv);
 
 event_base_dispatch(base);
 event_base_free(base);
 
 printf("exit\n");
 return 0;
}
```
