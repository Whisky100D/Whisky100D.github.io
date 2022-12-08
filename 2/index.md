# Python多线程卖票


<!--more-->

```python
#!/usr/bin/python3

import threading
import time
import queue
from colorama import Fore
from rich.progress import Progress

tt = 50
threads = []
q = queue.Queue()
# 控制最大可用窗口数
sem=threading.Semaphore(5)

def sale(sem,progress,task):
    while not q.empty():
        if threading.current_thread().name == "第1窗口":
            print(Fore.GREEN + "{}：卖出第{}张票".format(threading.current_thread().name,q.get()))
        elif threading.current_thread().name == "第2窗口":  
            print(Fore.MAGENTA + "{}：卖出第{}张票".format(threading.current_thread().name,q.get()))
        elif threading.current_thread().name == "第3窗口":  
            print(Fore.BLUE + "{}：卖出第{}张票".format(threading.current_thread().name,q.get()))
        elif threading.current_thread().name == "第4窗口":  
            print(Fore.CYAN + "{}：卖出第{}张票".format(threading.current_thread().name,q.get()))
        else:
            print(Fore.YELLOW + "{}：卖出第{}张票".format(threading.current_thread().name,q.get()))
        if not progress.finished:
            progress.update(task, advance=1)
        time.sleep(5)  
    sem.release()

def main():
    for i in range(tt):
        q.put(i+1)
    with Progress() as progress:
        task = progress.add_task("[cyan]Processing", total=tt)
        # 控制窗口数
        for j in range(20):
            sem.acquire()
            td = threading.Thread(target=sale,args=(sem,progress,task),name="第"+str(j+1)+"窗口")
            # 添加线程到线程列表
            threads.append(td)
            td.start()
        # 等待所有线程完成后退出主线程
        for t in threads:
            t.join()    
    print(Fore.RED +"票已卖完！！！")

if __name__=="__main__":
    main()
```

