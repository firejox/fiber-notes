# Fiber monitor and Fiber condition data structure (Hoare style)

```cpp
struct fiber_monitor_t {
    struct fiber_semaphore_t mutex = 1;
    struct fiber_semaphore_t next = 0;
    int next_count = 0;
};

void fiber_monitor_enter(struct fiber_monitor_t *m)
{
    fiber_semaphore_wait(&m->mutex);
}

void fiber_monitor_leave(struct fiber_monitor_t *m)
{
    if (m->next_count < 0)
        fiber_semaphore_signal(&m->next);
    else
        fiber_semaphore_signal(&m->mutex);
}

struct fiber_condition_t {
    struct fiber_semaphore_t sem = 0;
    int count = 0;
};

void fiber_condition_wait(struct fiber_condition_t *c, struct fiber_monitor_t *m)
{
    c->count = c->count + 1;
    fiber_monitor_leave(m);
    fiber_semaphore_wait(&cond->sem);
    c->count = c->count - 1;
}

void fiber_condition_signal(struct fiber_condition_t *c, struct fiber_monitor_t *m)
{
    if (c->count > 0) {
        m->next_count = m->next_count + 1;
        fiber_semaphore_signal(&c->sem);
        fiber_semaphore_wait(&m->next);
        m->next_count = m->next_count - 1;
    }
}
```
