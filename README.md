# MOE    
![LOGO](https://github.com/ianhom/MOE/blob/master/Documents/Pic/MOE_logo_V0_1e.png?raw=true)    
[中文说明](https://github.com/ianhom/MOE/blob/master/README_CHINESE.md) 
## Introduce
MOE is an event-driven scheduler system for 8/16/32-bit MCUs. Features with:   

 | Description   
----------------- | ---------------------------------------------   
**Event-driven** | Flexible event queue length, priority event supported.   
**Timer** | Useful ms-timer with callback.   
**Message** | Easy-use message API for communication between tasks, To-All-Task message with low RAM comsuption supported.   
**Debug** | Flexible debug print options for each task or module; Useful easy-assert.  
**Protothread** | Protothread is supported for application module.   

For more discussion, please join our QQ Group: **[475258651](https://jq.qq.com/?_wv=1027&k=41PrZvS)**   

## How to use
- **Step 1**: Port the MOE to your hardware, provide "**system clock in ms**" and "**poll function**", init and run MOE.  
```c
/* EXAMPLE CODE */
/* Function to get ms clock */
uint16 GetMsClock(void)
{
    return sg_u16SysClk;  /* System ms clock, increased every 1 ms in hardware timer interrupt */
}

/* Function to be Polled */
void Poll(void)
{
    /* Nothing or something you want to do by polling */
}

void main(void)
{
    .... /* Board init operation */
    MOE_Init(GetMsClock, Poll);
    MOE_Run(); 
    return;
}
```
- **Step 2**: **Create** your own tasks or **re-use** exist app tasks to build your application.   
```c
/* EXAMPLE CODE */
/* Task 1 */
uint8 Task_PT_Demo_Process(uint8 u8Evt, void *pPara)
{   
    PT_INIT();
    PT_BEGIN();
    /******************************************************************/
    MOE_MANDATORY_INIT();  /* Mandatory init, shout call it here only */
    /******************************************************************/
    while(1)
    {
        TASK_PT_DEMO_LED_On(LED_RED);
        PT_DELAY(1000);
        TASK_PT_DEMO_LED_Off(LED_RED);

        TASK_PT_DEMO_LED_On(LED_GREEN);
        PT_DELAY(1000);
        TASK_PT_DEMO_LED_Off(LED_GREEN);

        TASK_PT_DEMO_LED_On(LED_BLUE);
        PT_DELAY(1000);
        TASK_PT_DEMO_LED_Off(LED_BLUE);
    }
    PT_END();
    return SW_OK;
}
```   

```c
/* EXAMPLE CODE */
/* Task 2 */
uint8 Task_PT_Demo2_Process(uint8 u8Evt, void *pPara)
{    
    PT_INIT(); 
    PT_BEGIN();
    /******************************************************************/
    MOE_MANDATORY_INIT();  /* Mandatory init, shout call it here only */
    /******************************************************************/
    while(1)
    {
        DBG_PRINT("I am another Task!!\n");
        PT_DELAY(1000);
    }

    PT_END();
    return SW_OK;
}
```

- **Step 3**: **Register tasks** in **Project_Config.h** and continue other **configuration** in the same file.   
```c
#define LIST_OF_REG_TASK \
        REG_TASK(Task_PT_Demo_Proces)\
        REG_TASK(Task_PT_Demo2_Proces)
```
- **Step 4**: Run & Enjoy. :smile:   

## Useful Documents
 - [API Description(Chinese)](https://github.com/ianhom/MOE/blob/master/Documents/API_Description_Chinese.md)    
 - [Design Record(Chinese)](https://github.com/ianhom/MOE/blob/master/Documents/Design_Record.md) 


## Special Thanks 
- :tada:MOE Logo drawing by **Miss Cai Jianan**.:tada:
