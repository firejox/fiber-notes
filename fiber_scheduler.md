# Fiber Scheduler data structure

```cpp
struct fiber_scheduler_t {
    spin_lock_t global_lock;
    struct list_head fiber_list;
    struct list_head disposed_fiber_list;
};

void reschedule(struct fiber_scheduler_t *sched)
{
    struct list_head *it;
    struct fiber_t *fiber;
    lock_spin_lock(&sched->global_lock);
    
    it = sched->fiber_list.next;
    list_move_tail(it, &sched->fiber_list);
    fiber = container_of(it, struct fiber_t, link);
    
    fiber_insert_spin_unlocker(fiber, &sched->global_lock);
    fiber_switch(fiber);
}

void fiber_exit(struct fiber_scheduler_t *sched)
{
    struct list_head *it;
    struct fiber_t *fiber;
    
    lock_spin_lock(&sched->global_lock);
    
    it = sched->fiber_list.next;
    
    list_del(it);
    list_add_tail(it, &sched->disposed_fiber_list);
    
    fiber = container_of(it, struct fiber_t, link);
    
    fiber_insert_spin_unlocker(fiber, &sched->global_lock);
    fiber_switch(fiber);
}
```
