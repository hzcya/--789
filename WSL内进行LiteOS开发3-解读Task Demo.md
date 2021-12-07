对于LiteOS的入门，它的Demo几乎包含了LiteOS的所有模块，还有非常有学习价值的。

再来看一下"Kernel Task Demo"的运行结果：

********Hello Huawei LiteOS********

LiteOS Kernel Version : 5.1.0
build date : Dec  4 2021 16:22:43

**********************************
OsAppInit
cpu 0 entering scheduler
app init!
Hello, welcome to liteos demo!

Huawei LiteOS # 
Kernel task demo start to run.
Create high priority task successfully.
Create low priority task successfully.
Enter high priority task handler.
Enter low priority task handler.
High priority task LOS_TaskDelay successfully.
High priority task LOS_TaskSuspend successfully.
High priority task LOS_TaskResume successfully.
Kernel task demo finished.
在开始分析之前，首先需要补一点LiteOS中关于任务的知识点，对于任务有了基本的认识之后，接下来解读代码就很容易理解。

1.什么是任务？
从系统角度看，任务是竞争系统资源的最小运行单元。任务可以使用或等待CPU、使用内存空间等系统资源，并独立于其它任务运行。

2.任务的特点
支持多任务。
一个任务表示一个线程。
抢占式调度机制，高优先级的任务可打断低优先级任务，低优先级任务必须在高优先级任务阻塞或结束后才能得到调度。
相同优先级任务支持时间片轮转调度方式。
共有32个优先级[0-31]，最高优先级为0，最低优先级为31。
3.任务的状态
任务状态通常分为以下四种：

就绪（Ready）：该任务在就绪队列中，只等待CPU。
运行（Running）：该任务正在执行。
阻塞（Blocked）：该任务不在就绪队列中。包含任务被挂起（suspend状态）、任务被延时（delay状态）、任务正在等待信号量、读写队列或者等待事件等。
退出态（Dead）：该任务运行结束，等待系统回收资源。
任务状态迁移图：

task.jpg

好了，现在我们开始分析代码吧。

源码分析
源码路径：LiteOS/demos/kernel/api/los_api_task.c

这个文件看似很长，其实只有3个方法

1.主运行函数：TaskDemo(void)

2.高优先级任务运行的方法：HiTaskEntry(void)

3.低优先级任务运行的方法：LoTaskEntry(void)

代码中使用到的任务接口
接口名	描述
LOS_TaskLock	锁定调度
LOS_TaskUnlock	解锁调度
LOS_TaskCreate	创建任务
LOS_TaskDelay	延时任务
LOS_TaskSuspend	挂起任务
LOS_TaskResume	重启任务
LOS_TaskDelete	删除任务
程序流程
从TaskDemo(void)函数开始执行

为了防止在创建任务时，被创建的任务优先级高于主函数的优先级，必须先锁任务调度

    LOS_TaskLock();
创建高优先级任务，因为现在处理锁任务调度状态，刚创建的任务不会立刻执行。

    taskInitParam.pfnTaskEntry = (TSK_ENTRY_FUNC)HiTaskEntry;
    taskInitParam.usTaskPrio = HI_TASK_PRIOR;
    taskInitParam.pcName = "TaskDemoHiTask";
    taskInitParam.uwStackSize = LOSCFG_BASE_CORE_TSK_DEFAULT_STACK_SIZE;
#ifdef LOSCFG_KERNEL_SMP
    taskInitParam.usCpuAffiMask = CPUID_TO_AFFI_MASK(ArchCurrCpuid());
#endif
    ret = LOS_TaskCreate(&g_demoTaskHiId, &taskInitParam);
    if (ret != LOS_OK) {
        LOS_TaskUnlock();
        printf("Create high priority task failed.\n");
        return LOS_NOK;
    }
    printf("Create high priority task successfully.\n");
再创建低优先级任务

    taskInitParam.pfnTaskEntry = (TSK_ENTRY_FUNC)LoTaskEntry;
    taskInitParam.usTaskPrio = LO_TASK_PRIOR;
    taskInitParam.pcName = "TaskDemoLoTask";
    taskInitParam.uwStackSize = LOSCFG_BASE_CORE_TSK_DEFAULT_STACK_SIZE;
    ret = LOS_TaskCreate(&g_demoTaskLoId, &taskInitParam);
    if (ret != LOS_OK) {
        /* delete high prio task */
        if (LOS_OK != LOS_TaskDelete(g_demoTaskHiId)) {
            printf("Delete high priority task failed.\n");
        }
        LOS_TaskUnlock();
        printf("Create low priority task failed.\n");
        return LOS_NOK;
    }
    printf("Create low priority task successfully.\n");
在创建任务完成之后，解锁任务调度

LOS_TaskUnlock();
此刻程序开始按任务的优先级，先执行高优先级任务，输出信息

Enter high priority task handler.
在HiTaskEntry(void)函数内继续执行至：

ret = LOS_TaskDelay(DELAY_INTERVAL1);
延时5个Tick，延时后任务挂起，开始执行剩余任务中低先级任务（LoTaskEntry）。

进入LoTaskEntry函数，输出信息：

Enter low priority task handler.
在LoTaskEntry(void)函数内继续执行至：

ret = LOS_TaskDelay(DELAY_INTERVAL2);
延时10个Tick,延时后任务挂起，这时HiTaskEntry的延时结束，又切换回HiTaskEntry，从“ret = LOS_TaskDelay(DELAY_INTERVAL1);”所在行之后，继续向下执行，打印信息：

High priority task LOS_TaskDelay successfully.
接下来，执行到这一步：

ret = LOS_TaskSuspend(g_demoTaskHiId);
HiTaskEntry被挂起，程序开始在剩余任务中查询可以被执行的任务。

此时LoTaskEntry还在执行延时操作的命令，没有可以执行的任务。整个程序会处理延时等待中。

在LoTaskEntry执行延时操作结束之后，从阻塞状态变成就绪状态，被程序调用从原来的执行点继续向下执行，输出信息：

High priority task LOS_TaskSuspend successfully.
再接着LoTaskEntry将HiTaskEntry恢复：

ret = LOS_TaskResume(g_demoTaskHiId);
此时LoTaskEntry被阻塞，HiTaskEntry重新抢占任务调度，从上次中断的位置继续向下执行，输出信息：

High priority task LOS_TaskResume successfully.
最后HiTaskEntry运行完毕，从任务调度中被清除

LoTaskEntry从上次中断的位置继续向下执行，至程序结束，最终的输出结果如文章开头所示。
