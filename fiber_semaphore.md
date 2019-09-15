# Fiber Semaphore data structure

```cpp
/*
 * [cur_sched] : current fiber scheduler
 */

struct fiber_semaphore_t {
    spin_lock_t mutex;
    struct list_head wait_list;
    int count;
};

void fiber_semaphore_wait(struct fiber_semaphore_t *s)
{
    struct list_head *it;
    struct fiber_t *fiber;
    
    lock_spin_lock(&s->mutex);
    
    s->count = s->count - 1;
    if (s->count < 0) {
        lock_spin_lock(&[cur_sched]->global_lock);
        
        it = cur_sched->fbier_list.next;
        
        list_del(it);
        list_add_tail(it, &s->wait_list);
        
        fiber = container_of(it, struct fiber_t, link);
        
        fiber_insert_spin_unlocker(fiber, &s->mutex);
        fiber_insert_spin_unlocker(fiber, &[cur_sched]->global_lock);
        fiber_switch(fiber);
    } else {
        unlock_spin_lock(&s->mutex);
    }
}

void fiber_semaphore_signal(struct fiber_semaphore_t *s)
{
    struct list_head *it;
    struct fiber_t *fiber;
    
    lock_spin_lock(&s->mutex);
    
    s->count = s->count + 1;
    if (s->count <= 0) {
        lock_spin_lock(&[cur_sched]->global_lock);
        
        it = s->waiting.next;
        list_del(it);
        list_add_tail(it, &[cur_sched]->fiber_list);
        
        fiber = container_of(it, struct fiber_t, link);
        
        fiber_insert_spin_unlocker(fiber, &s->mutex);
        fiber_insert_spin_unlocker(fiber, &[cur_sched]->global_lock);
        fiber_switch(fiber);
    } else {
        unlock_spin_lock(&s->mutex);
    }

}
```
