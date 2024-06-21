## 通用代码框架
```python
# multiprocessing为python内置模块
from multiprocessing.dummy import Pool  # 非常容易漏掉dummy
import threading

pool = Pool(10)  # 开启线程数量，这里设置10个
lock = threading.Lock()  # 线程锁，多个线程同时访问一个对象时需要使用，比如全局变量
df = pd.DataFrame()

def multi_process(param):  #多线程函数
    # 代码......
    df0 = ...
    lock.acquire()  # 上锁
    df = df.append(df0)  # 例子，此处为一对多，多个线程修改一个df（全局），容易出错，应该上锁
    lock.release()  # 解锁

    # 代码......

pool.map(multi_process, list_params)  # 开启多线程运行
# multi_process为多线程函数名
# list_params为可迭代对象，比如列表
# 如果要传多个参数，可用字典型列表等

pool.close()
pool.join()
# 调用join之前，先调用close函数，否则会出错。
# 执行完close函数后不会有新的进程加入到pool，join函数等待所有子进程结束
```
## 面向对象形式的示例代码
```python
import requests
from multiprocessing.dummy import Pool  # 非常容易漏掉dummy
import time
import threading 


class Test(object):

    def __init__(self):
        self.pool = Pool(10)  # 开启线程数量，这里设置10个
        self.total = 0  # 全局变量
        self.lock = threading.Lock()  # 线程锁，多个线程同时访问一个对象时需要使用，比如全局变量

    def get_data(self, num):  # 模拟网络请求
        time.sleep(1)
        return num

    def run_pool(self, nums):
        
        list_num = [n for n in range(1, nums+1)]

        def run(num):
            n = self.get_data(num)

            self.lock.acquire()
            self.total = self.total + n
            self.lock.release()

        self.pool.map(run, list_num)
        self.pool.close()
        self.pool.join()

        return self.total


if __name__ == '__main__':
    t = Test()
    total = t.run_pool(10)
    print(total)
```