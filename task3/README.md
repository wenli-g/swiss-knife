# Swissknife for computer systems
## Task 3 - System Benchmarking
### A - KV store / database
Benchmarking is one of the important key task when there is
a wide range of databases and kv-stores available in order to
meet different demands of industry. We run different benchmarks to see the behaviour of our hardware, and also to analyse how our database and application perform under heavy
loads. This part of the assignment illustrates comparisons in
performance of different database systems under different
setups. In basic task, 2 key-value stores (Rocksdb and Redis)
and in bonus task, 2 db engines (myRocks, InnoDB) running
on MYSQL have been used in the experiment.


## Please note this project has been run and tested on Ryan server provided by TUM.
#### Steps for deployment: (script will prompt to enter results version e.g. v1, v2,v3)
<ul>
  <li>Run script file <code>kvstore.sh</code>  in <code>task3/kvstore</code> directory. (script will prompt for an user input for "result-version"). It will run both basic task and exploration task.</li>
  <li>It can also be run seperately. For basic task, run script file <code>ycsb.sh</code>  in <code>task3/kvstore/basic</code> directory and for exploration task, run script file <code>rockdb.sh</code> in <code>task3/kvstore/exp</code> directory.</li>
  <li>Results directories for basic and exploration tasks are <code>task3/kvstore/basic/results/"result-version"</code> and <code>task3/kvstore/exp/bonus_results/"result-version"</code></li>
</ul>

### B- Filesystem
In this section, we are going to benchmark 2 different filesystems i.e. EXT4 and BTRFS. There are many differences between these 2 file systems. Major difference is that Ext4 is a journaling file and Btrfs is a modern copy-on-write(CoW) with some additional advanced features. Btrfs also provides a built-in RAID support whereas Ext4 needs to use a 3rd party manager for logical volumes. Btrfs is space efficient and provides filesystem level deduplication that means it removes duplicate copies of data. Btrf also provides snapshots, extensive checksums, scrubbing, self-healing data, and many more useful improvements to ensure data integrity.

#### Steps for deployment: (script will prompt to enter results version e.g. v1, v2,v3)
<ul>
  <li>Run script file <code>filesystem.sh</code>  in <code>task3/filesystem</code> directory. (script will prompt for an user input for "result-version"). It will run both basic task and exploration task.</li>
  <li>It can also be run seperately. First run  <code>format.sh </code> to format for ext4 and btrfs filesystems <code>task3/filesystem</code> directory. For basic task, run script file <code>fio.sh</code> in <code>task3/filesystem</code> directory and for exploration task, run script file <code>phoronix.sh</code> in <code>task3/filesystem</code> directory.</li>
  <li>Results directories for basic tasks are <code>task3/filesystem/results_fio/random_"result-version"</code> and <code>task3/filesystem/results_fio/sequential_"result-version"</code>and and for exploration task, make sure to copy the URL when it finishes for online results on openbenchmarking</li>
</ul>

### C - Networking
Having already mentioned the relevance of benchmarking we now want to focus on the importance of network benchmarking. Benchmarking is at its core the comparison of processes under a certain criteria - most likely performance. Thus, network benchmarking enables us to have an overview of what is the performance of our explored network. Furthermore, it might provide us an insight of how our network compares to other similar ones. Logically, this is considered a good practice procedure since these measurements become more and more crucial as we approach a much more competent environment and as we get a glimpse at what should be the standard behavior. The former being more often the reason why network benchmarking has become a widely researched field. 

#### Steps for deployment:

For a detailed description of the instructions and results please head over to the following Notion site. 
https://adrianm.notion.site/Task-3-Network-Benchmarking-with-iPerf-Team-F-d67c238499744279b0d908b17ceb5166


### D - MultiCore

To improve the performance of processors, manufacturers have been releasing more multi-core machines. Having multiple cores, each core can handle a different thread simultaneously. In this section, we will dive into multi-core benchmarking, with two benchmark suites: Phoenix 2.0, PARSEC 3.0.
Phoenix is a programming API based on Google's MapDeduce model. It parallelizes MapDeduce applications by 4 steps: dividing input data into chunks, parallelizing Map tasks, parallelizing Reduce tasks, and merging reduce queues into single queue. Phoenix also provides some sample applications. In the basic task, we use these applications of Phoenix 2.0 for multi-cores benchmarking. Later in the bonus task, we have tested with benchmark suite PAR-SEC 3.0

#### Steps for deployment:

<ul>
  <li>Run script file <code>run_multicore_basic.sh</code>  in <code>task3/multicore</code> directory. It will run for basic task and create output files in <code>task3/multicore/results</code> directory.</li>
  <li>Run script file <code>run_multicore_exp.sh</code>  in <code>task3/multicore</code> directory. It will run for exploration task and create output files in <code>task3/multicore/results</code> directory.</li>
</ul>

__________________________________________________________________________________________________________________________________________________________________
## Results
### A - KV store / database
#### -- Basic Task
![image](https://user-images.githubusercontent.com/76809539/149373511-e6d51c7d-617f-40d4-a71e-e705c4ef1e37.png)
<br>
We merged our results for both comparisons. In figure 1, 1 for heavy update workload, read latency of RocksDB is significantly lower than Redis single instance and almost identical to redis cluster. Due to high number of connections, redis single instance is having higher latency. For update latency graph, plot shows that the redis cluster has performed better than both rocksDB(for higher throughput) and redis. Fault tolerance ability and how the load is distributed across cluster nodes in redis cluster is the reason of having lower latency. It also shows that for both reads and updates, rocksdb showed better throughput and lower latency than the redis cluster till offered throughput reached 22k-25k/sec. In each case, we increased the offered throughput until the actual throughput stopped increasing. <br>
![image](https://user-images.githubusercontent.com/76809539/149373049-0ab58bfc-ec4e-4b7e-8388-f49620547f44.png) <br>
Figure 2 shows the read latency with ReadOnly workload and Figure 2: Read Only Workload Zipfan request distribution. The results are pretty much similar to Heavy update workload with a slight shift of throughput/sec from 25k to 30k for redis cluster to perform better than rocksDB. Redis single instance again is the slowest amongst all DBs. <br>
![image](https://user-images.githubusercontent.com/76809539/149373140-c5f77ac3-2645-4eb4-99d0-dc2285113a20.png)<br>
Figure 3 shows latency curves under Latest access distribution which means that the record entered most recently has the highest probability to be accessed. Read latency curves are same as in Readonly workload but in insert case, rocksdb is more efficient throughout the offered throughput. It might be due to the fact that rocksdb is a library embedded kv store and does not require any network. <br>

#### -- Bonus Task
![image](https://user-images.githubusercontent.com/76809539/149373618-89516368-3960-47a9-9b09-7dfde76a138a.png)<br>
![image](https://user-images.githubusercontent.com/76809539/149373664-d334d880-4e8d-47f4-9881-7cb5b72fc9de.png)<br>
Figure 4 and figure 5 show a comparison between innodb and myrocks engines with 32 and 64 threads in terms of transactions per 5 seconds. We can see that myrocks engine has outperformed InnoDB almost by 5 times. MyRocks trx stays between 5k to 6k for both number of threads where InnoDB averaged out between 1.4k and 1.5k trx/5sec. May be InnoDB would outrun Myrocks by increasing the size of the warehouses and tables in it. In our experiment, both of them showed a stable performance with not much variations i.e. performance drop. In conclusion, Rocksdb is built with a great compression ratio that is optimized for SSDs and flash storage. Rocksdb potentially makes itself a great candidate for cloud databases and it will also decrease the cost of deployment given that cost of I/O and memory.<br>

### B- Filesystem
#### -- Basic Task
![image](https://user-images.githubusercontent.com/76809539/149374740-3306c51a-ab57-4714-8e16-b4b68ff3c741.png)<br>
![image](https://user-images.githubusercontent.com/76809539/149374328-6cde5804-b2f7-49a7-9a2f-733b2e05a15c.png)<br>
In plots 6 and 7, we changed block size with random read with iodirect enabled in fig 6 and disabled in fig 7 and fixed IO depth at 16 and recorded the bandwidth for each case. It can be seen that the ext4 filesystem performed a lot better than btrfs except at when block size is 4K. Ext4 peaked at around 7000MB/s when the blocksize was 2m while Btrfs could do max of 2000Mb/s when block size was 64k. This might be due to the fact that btrfs validates first if the data is not corrupted. It looks for the checksums of blocks while it reads them and also the reason of it being slower is because it is a CoW filesystem.<br>
![image](https://user-images.githubusercontent.com/76809539/149374418-2dc063b9-782e-49df-a49d-7bb74659491d.png)<br>
In figure 8, we changed the IO type to sequential write and observed a lower performance by both filesystems but still throughput of ext4 is 2 times of that of btrfs.<br>
![image](https://user-images.githubusercontent.com/76809539/149374468-7c3258fd-0f09-4928-8ebf-1e4def5af54b.png)<br>
In figure 9, we tried to vary IO depth with random read and btrf again could not match the bw of ext4. Ext4 is exceptionally good for storage processes. We had performed couple of other comparisons too i.e. Random write and Sequential Read that can be seen in our github repository. To conclude, we can say that Btrfs has its own attraction with its different advanced features like snapshots, built-in Raid etc. and is rapidly growing while Ext4 has better performance, more stable and reliable. It is really a matter of requirement in choosing between these filesystems.<br>
#### -- Bonus Task
After running the suite, it performed 19 benchmarks with different settings. Few of them are discussed below. (All results can be seen from index.html in \verb|results_phoronix| directory.<br>
![image](https://user-images.githubusercontent.com/76809539/149375339-88613b0d-4eb1-4b8c-96d3-d09587b1898c.png)<br>
Figure 10 shows the result of Random read with block size 4k and 2m by FIO. The results are pretty similar to what we got in our basic task.<br>
![image](https://user-images.githubusercontent.com/76809539/149376240-8f11fb1c-25e9-4507-aa3f-0f4e3c02e7ba.png)<br>
Figure 11 shows IOPS of ext4 with the same settings. IOPS surged up to 210k when the blocksize is increased to 2mb. Its not surprising because the block size is simply the maximum limit a block can be filled up with transactions. <br>
![image](https://user-images.githubusercontent.com/76809539/149376310-91ee501a-becd-4dfd-aede-fee3de4af23c.png)
<br>
In figure 12, we can look into FS-mark benchmark results. It changed the number of threads from 1 to 4 and number of file/s rose to more than double than with 1 thread. <br>

### C- Networking
#### -- Basic Task
![image](https://user-images.githubusercontent.com/76809539/149411931-ba64f669-7853-4993-813f-de24517a087d.png)<br>
In figure 13, we can look at the increment of datarates on both transfer and bitrate when running the TCP benchmark with different bandwiths. Figure 14 illustrates the jitter and dataframe amount differences. <br>
![image](https://user-images.githubusercontent.com/76809539/149412243-35c68de9-2af2-4b85-8874-406231997e64.png)<br>
![image](https://user-images.githubusercontent.com/76809539/149412218-ddeb2cce-d6a2-45cb-9b75-1dbb3b60f860.png)<br>
The change in window size [Figure 15], whilst running under TCP, causes a first considerable change when going from 8K to 128K yet this change become neglectable as the window size grows - the cwmd keeps growing. <br>
![image](https://user-images.githubusercontent.com/76809539/149412317-58c57fb9-a6b4-4051-a9a0-99c58df9d49e.png)<br>
When analyzing what happens when running the reverse mode, we get the following illustration from Figure 16 and 17. Here we divided the graphs between the "default" and reverse mode from TCP and UDP accordingly. <br>
![image](https://user-images.githubusercontent.com/76809539/149412378-356e00a7-665b-4755-be27-1af96410ce12.png)<br>
Lastly, Figure 18 displays the change in average transfer and bitrate while running with TCP (since there is no difference at all when applying parallel streams using UDP). <br>

### D - Multicore
#### -- Basic Task
![image](https://user-images.githubusercontent.com/76809539/149424361-0caa6d00-a372-40dd-b839-387b1348e51c.png)<br>
In figure 19, except kmeans, all the other workloads show a significant performance improvement. With the increasing of cores, the performances of the workloads histogram and string match increase accordingly, while the workloads linear regression and word count already reach their highest performance with 4 cores.<br>
![image](https://user-images.githubusercontent.com/76809539/149424468-6d4b20aa-78c2-461e-93d4-573721a09421.png)<br>
In figure 20, the speedups of most workloads are higher with larger data size, but the workload linear regression gains the lowest speedup with medium data size.<br>  
#### -- Bonus Task
![image](https://user-images.githubusercontent.com/76809539/149424545-feba4d18-cb58-4390-b6e8-f28d4b245f7f.png)<br>
![image](https://user-images.githubusercontent.com/76809539/149424589-94016f75-6995-4f9e-9bbd-0a49d4af2721.png)<br>
As shown in Figure 21 and 22, all the workloads gain a speedup (> 1). Like the basic task, the performances are not always related to the number of threads or the size of data. We suspect that the performances do not always grow up with the number of cores, due to multiprocessing overhead.<br>
