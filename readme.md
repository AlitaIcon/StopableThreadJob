### 可停止线程任务管理器

    参考Java线程实现，利用异常中止任务运行

### Quick Start

```python
import time
import datetime
from loguru import logger

from StopableThreadJob.job_manager import JobManager

if __name__ == '__main__':
    def slow_func( name):
        for i in range(5):
            logger.info(f"{name} -- {datetime.datetime.now()}")
            time.sleep(1)


    job_manager = JobManager()
    # 删除未添加任务
    job_manager.remove_job('2')
    for pid in range(6):
        logger.info(f"添加任务: {pid}")
        job_manager.add_job(target=slow_func, args=(pid,), job_id=f'{pid}')
    time.sleep(1)
    job_manager.start_job()
    # 删除已添加运行中任务
    job_manager.remove_job('1')
    # 删除已添加未运行中任务
    job_manager.remove_job('4')
    time.sleep(5)
    # 删除运行完成任务
    job_manager.remove_job('0')
    job_manager.print_current_job()
    print(job_manager.job_store)
    for i in [0, 1, 2, 4]:
        logger.info(f"添加任务: {i}")
        job_manager.add_job(target=slow_func, args=(i,), job_id=f'{i}')
    job_manager.print_current_job()
    job_manager.start_job()
    time.sleep(6)
    print(job_manager.job_store)
    job_manager.print_current_job()
    time.sleep(30)
```