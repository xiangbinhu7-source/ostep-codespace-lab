## Q1
```
- Prediction / 预测: 10，CPU利用率：100%
- Reasoning / 理由: 预测总时间、CPU利用率与模拟器运行结果一致。两个进程都只使用CPU，没有IO阻塞，CPU不会空闲。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5        RUN:cpu         READY             1          
  6           DONE       RUN:cpu             1          
  7           DONE       RUN:cpu             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1          

Stats: Total Time 10
Stats: CPU Busy 10 (100.00%)
Stats: IO Busy  0 (0.00%)
- Analysis / 分析: 预测总时间、CPU利用率与模拟器运行结果一致。两个进程都只使用CPU，没有IO阻塞，CPU不会空闲。
```
## Q2
```
- Prediction / 预测: 8，CPU利用率：50.00%
- Reasoning / 理由: PID0先执行4条CPU指令，之后PID1执行IO；IO等待期间CPU空闲，总时间8。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5           DONE        RUN:io             1          
  6           DONE       BLOCKED                           1
  7           DONE       BLOCKED                           1
  8           DONE       BLOCKED                           1
  9           DONE       BLOCKED                           1
 10           DONE       BLOCKED                           1
 11*          DONE   RUN:io_done             1          

Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
- Analysis / 分析: 预测总时间、CPU利用率与模拟器运行结果一致。PID1进行IO时，两个进程都无法占用CPU，CPU空闲。
```
## Q3
```
- Prediction / 预测: 7，CPU利用率：85.71%
- Reasoning / 理由: PID0执行IO指令后进入阻塞，CPU切换到PID1执行4条CPU指令；PID1执行完毕后，CPU等待PID0的IO完成，总时间为7。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
- Analysis / 分析: 预测总时间、CPU利用率与模拟器运行结果一致。PID0发起IO阻塞后，CPU调度PID1运行；PID1提前运行结束，剩下时间CPU等待IO完成。
```
## Q4
```
- Prediction / 预测: 7，CPU利用率：85.71%
- Reasoning / 理由: PID0执行IO阻塞，系统仅在进程结束时切换，PID1运行4条CPU指令。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1          
 11           DONE       RUN:cpu             1          

Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
- Analysis / 分析: PID0执行IO阻塞，系统仅在进程结束时切换，PID1运行4条CPU指令。
```
## Q5
```
- Prediction / 预测: 7，CPU利用率：85.71%
- Reasoning / 理由: PID0发起IO时切换至PID1，PID1执行4条CPU指令。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
- Analysis / 分析: 预测与实际一致。SWITCH_ON_IO在IO启动时切换进程，充分利用CPU。
```
## Q6
```
- Prediction / 预测: 7，CPU利用率：85.71%
- Reasoning / 理由: 触发IO时切换进程；IO完成后不立刻运行IO进程，继续运行当前进程
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
- Analysis / 分析: 预测与实际一致。IO完成后不会抢占CPU，等到当前进程结束才切换。
```
## Q7
```
- Prediction / 预测: 7，CPU利用率：85.71%
- Reasoning / 理由: PID0 IO完成后立刻抢占CPU运行，不再等待当前进程结束。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
- Analysis / 分析: 预测与实际一致。IO_RUN_IMMEDIATE 在IO完成时马上调度该进程。
```
## Q8
```
- Prediction / 预测: 不同随机种子会生成不一样的指令序列，总时间和CPU利用率会随种子变化
- Reasoning / 理由: 3:50表示每个进程3条指令，每条50%概率为CPU或IO；随机种子决定指令序列。
- Verified result / 验证结果:
- 1
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1          

Stats: Total Time 15
Stats: CPU Busy 8 (53.33%)
Stats: IO Busy  10 (66.67%)
- 2
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1          

Stats: Total Time 16
Stats: CPU Busy 10 (62.50%)
Stats: IO Busy  14 (87.50%)
- 3
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*       RUN:cpu         READY             1          
 10           DONE   RUN:io_done             1          
 11           DONE        RUN:io             1          
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16           DONE       BLOCKED                           1
 17*          DONE   RUN:io_done             1          
 18           DONE       RUN:cpu             1          

Stats: Total Time 18
Stats: CPU Busy 9 (50.00%)
Stats: IO Busy  11 (61.11%)
- Analysis / 分析: 预测与实际一致。不同随机种子产生不同指令序列，调度结果不同。
```