# Blog 11 [11.22.19]

# Concurrency programming in Python

A slight variation on parallel programming, concurrent programming also serves to help cut down the time associated with a program in runtime.

##### sidenote
Technically speaking: while it may look like parallel processing, it technically is not. Concurrency as shown in this example technically does not run the tasks " at the same time ". Rather: the module we are using assigns each thread a task that will eventually reach a waiting state. Once this happens, the next thread will start its task. 
Additionally, this blog post assumes python3.4 and above.

### The magic of concurrency

Say we have a for loop that processes HTTP requests for later.
```
import http.client
import socket
port = 80
giantListofURLs=[foo,bar,foo1,bar1......foo(N),bar(N)]
arbitrarylist=[]
for i in giantListofURLs: 
	url = i
	try:
		connection = http.client.HTTPConnection(url,port,timeout=5)
		connection.request('GET',url)
		response = connection.getresponse()
		arbitrarylist.append(response)
 ```
 In a "linear" program, this simply would  iterate through each individual item on the list. 
 Each time you iterate "i", the entire function must complete  before the next iteration.
 Given a large enough list, you'd spend hours gathering web requests. This is where the magic happens.

```
import http.client
import socket
from concurrent.futures import ThreadPoolExecutor as PoolExecutor 
port = 80 
giantListofURLs=[foo,bar,foo1,bar1......foo(N),bar(N)] 
arbitrarylist=[]
def getResponseCodes(url):
	try:
		connection = 
		http.client.HTTPConnection(url,port,timeout=5) 
		connection.request('GET',url) response = connection.getresponse() 
		arbitrarylist.append(response)
	
with PoolExecutor(max_workers=6) as executor:
	for _ in executor.map(getResponseCodes,giantListofURLs,):
	pass
```
Notice now that the section which was previously a for loop is gone. It is now a function that performs the same task without any sort of iteration.
We use the "ThreadPoolExecutor" library from the concurrent futures module in Python to build what's called a "thread pool" which is an arbitrary number of threads of execution that are available to perform a specified task.
Here we specify 6 workers. i.e. 6 maximum threads of execution for this task. I've read it recommended to go 2 or 3 times your processor number max, but I am not sure about best practices regarding this.

What ends up happening in the end is that the tasks are mapped between these 6 threads like so:
```
thread 1: getResponseCodes(giantListofURLs[0])
thread 2: getResponseCodes(giantListofURLs[1])
thread 3: getResponseCodes(giantListofURLs[2])
thread 4: getResponseCodes(giantListofURLs[3])
thread 5: getResponseCodes(giantListofURLs[4])
thread 6: getResponseCodes(giantListofURLs[5])
```
and as soon as the thread finishes, it takes on the next iteration of the list.
```
thread 1: getResponseCodes(giantListofURLs[6])
thread 2: getResponseCodes(giantListofURLs[7])
thread 3: getResponseCodes(giantListofURLs[8])
thread 4: getResponseCodes(giantListofURLs[9])
thread 5: getResponseCodes(giantListofURLs[10])
thread 6: getResponseCodes(giantListofURLs[11])
```
Note that these do not necessarily finish in order. They simply wait until their iteration of the task is done and then take the next available element in the list. Say in the above example that thread 5 finishes first because it resolved the hostname first. It would then look like so:
```
thread 1: getResponseCodes(giantListofURLs[6])
thread 2: getResponseCodes(giantListofURLs[7])
thread 3: getResponseCodes(giantListofURLs[8])
thread 4: getResponseCodes(giantListofURLs[9])
thread 5: getResponseCodes(giantListofURLs[12])
thread 6: getResponseCodes(giantListofURLs[11])
```
Thread 1 and 4's site is still taking time to finish the GET request, where as 2, 3, and 6 finish their execution.
```
thread 1: getResponseCodes(giantListofURLs[6])
thread 2: getResponseCodes(giantListofURLs[13])
thread 3: getResponseCodes(giantListofURLs[14])
thread 4: getResponseCodes(giantListofURLs[9])
thread 5: getResponseCodes(giantListofURLs[12])
thread 6: getResponseCodes(giantListofURLs[15])
```
At the end, you'll still get the list of response codes that you originally needed.

This is magnitudes faster than your linear version of the code. 
We can time this, like so:
```
start_time = time.time()
with PoolExecutor(max_workers=6) as executor:
for _ in executor.map(getResponseCodes,giantListofURLs,):
	pass
print("--- %s seconds ---" % (time.time() - start_time))
```
![image](https://user-images.githubusercontent.com/20525440/69475302-561df400-0d80-11ea-9679-cae5436e1464.png)
Here is the original execution time of the linear example.

![image](https://user-images.githubusercontent.com/20525440/69475313-6cc44b00-0d80-11ea-8d62-1d14d240ab8b.png)
This is the same code running with 4 works in the threadpool. As you can see, the number almost directly ties in to how much faster the program runs. 

Like with all forms of parallelism, we do run the risk of race conditions, so we need to limit the amount done by the task.

Say for example, we needed to append not only IP, but the machine name, OS, etc.
With multiple threads of execution, if you're not careful, the task in one thread to append the list of IPs might not finish in time before another thread starts appending to the list. This will result in incorrect mapping of  machine names to IPs.

This risk increases with the number of max workers in the threadpool, so take that into consideration when designing your program.
