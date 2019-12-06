# Fiber data structure

```cpp
struct fiber_t {
    void *stack_top;
    struct list_head link;
};

void fiber_suspend(struct fiber_t *fiber)
{
    save_registers;
    save_fiber_resume_function_pointer_into_stack;
    save_some_important_informations; // Optional
    swap_stack_top;
    restore_some_important_informations; // Optional
    return; // return to the stack top address
}

void fiber_resume(void)
{
    restore_registers;
}

void fiber_switch(struct fiber_t *fiber)
{
    fiber_suspend(fiber);
}
```
