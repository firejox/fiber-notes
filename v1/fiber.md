# Fiber data structure

```cpp
struct fiber_t {
    void *stack_top;
    struct list_head link;
};

void fiber_switch(struct fiber_t *fiber)
{
    save_registers;

    swap_stack_top;

    restror_registers;
}
```
