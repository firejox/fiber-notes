# Fiber channel data structure

```cpp
struct fiber_channel_t {
    struct fiber_monitor_t m;
    struct fiber_condition_t sender;
    struct fiber_condition_t receiver;
    void *data = NULL;
};

void fiber_channel_send(struct fiber_channel_t *ch, void *data)
{
    fiber_monitor_enter(&ch->m);
    
    if (ch->data != NULL)
        fiber_condition_wait(&ch->sender, &ch->m);
    
    ch->data = data;
    fiber_condition_signal(&ch->receiver, &ch->m);
    
    fiber_monitor_leave(&ch->m);
}

void *fiber_channel_receive(struct fiber_channel_t *ch)
{
    void *temp;
    
    fiber_monitor_enter(&ch->m);
    
    if (ch->data == NULL)
        fiber_condition_wait(&ch->receiver, &ch->m);
    
    temp = ch->data;
    ch->data = NULL;
    fiber_condition_signal(&ch->sender, &ch->m);
    
    fiber_monitor_leave(&ch->m);
    
    return temp;
}
```
