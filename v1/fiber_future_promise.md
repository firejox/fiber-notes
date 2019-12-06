# Fiber future and Fiber promise data structure

```cpp
struct fiber_future_t {
    struct fiber_semaphore_t sem = 0;
    void *data;
};

struct fiber_promise_t {
    spin_lock_t mutex;
    struct fiber_future_t future;
};

void* fiber_future_get_data(struct fiber_future_t *f)
{
    if (f->data == NULL) {
        fiber_semaphore_wait(&f->sem);
        fiber_semaphore_signal(&f->sem);
    }
    
    return f->data;
}

void fiber_promise_set_data(struct fiber_promise_t *p, void *data)
{
    if (p->future.data == NULL) {
        lock_spin_lock(&p->mutex);
        
        if (p->future.data == NULL) {
            p->future.data = data;
            unlock_spin_lock(&p->mutex);
            
            fiber_semaphore_signal(&p->future.sem);
        } else {
            unlock_spin_lock(&p->mutex);
        }
    }
}
```
