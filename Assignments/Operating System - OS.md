---
title: Operating System - OS
updated: 2023-02-06 11:04:07Z
created: 2023-02-02 04:08:07Z
---

<h1>
<br>
<center>Assignment # 02</center>
 Ghufran Mehmood <br>212400008<br> Operating System
<br>
</h1>

 ## a. Why we need to move the process in between queues?
-	In order to prevent starvation, it is necessary to implement a system of multiple queues for processes. If a process is unable to complete within the designated time quantum of a higher priority queue, it should be transferred to a lower priority queue in order to ensure its completion.

## b. What is starvation?
- Starvation arises as an issue when high priority processes continuously execute, causing low priority processes to be blocked for an indefinite period of time.
- **Solution**
	- The solution to starvation is to implement a scheduling algorithm that ensures a balance between high and low priority processes. This can be achieved by assigning time quantums to processes, periodically aging the priority of processes, or employing a combination of scheduling algorithms. By implementing such measures, starvation can be effectively prevented.

## 1. <center>Short Job Remaining Time First</center>

<center>

| Process | AT  | BT  | CT  | TAT | WT  | RT  |
| --- | --- | --- | --- | --- | --- | --- |
| P<sub>0</sub> | 7   | 12  | 31  | 31-7=24 | 24-12=12 | 19-7=12  |
| P<sub>1</sub> | 1   | 22  | 73  | 73-1=72 | 72-22=50 | 51-1=50  |
| P<sub>2</sub> | 5   | 38  | 206 | 206-5=201 | 201-38=163 | 168-5=163 |
| P<sub>3</sub> | 6   | 27  | 100 | 100-6=94 | 94-27=67 | 73-6=67  |
| P<sub>4</sub> | 0 | 19 | 19 | 19-0=19 | 19-19=0 | 0-0=0 |
| P<sub>5</sub> | 2   | 35  | 168 | 168-2=166 | 166-35=131 | 133-22=131 |
| P<sub>6</sub> | 18  | 20  | 51  | 51-18=33 | 33-20=13 | 31-18=13  |
| P<sub>7</sub> | 13  | 33  | 133 | 133-13=120 | 120-22=87 | 100-13=87 |
| P<sub>8</sub> | 11  | 39  | 245 | 245-11=234 | 234-39=195 | 206-11=195 |

</center>

### <center>Gant Chart</center>

<center>

| P<sub>4</sub> |     |     |     | P<sub>0</sub> | P<sub>6</sub> | P<sub>1</sub> | P<sub>3</sub> | P<sub>7</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>8</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
</center>
<br>

- **Average TAT = 963/9 = 107**
- **Average Waiting Time = 718/9 = 79.7**
- **Average Response Time = 718/9 = 79.7**
- **Throughput at 130ms = 5**

<center>-------------------------------------------------------------------------------------------</center>

## 2. <center>First Come First Serve</center>

<center>

| Process | AT  | BT  | CT  | TAT | WT  | RT  |
| --- | --- | --- | --- | --- | --- | --- |
| P<sub>0</sub> | 7   | 12  | 153 | 153-7=146 | 146-12=134 | 141-7=134 |
| P<sub>1</sub> | 1   | 22  | 41  | 41-1=40 | 40-22=18 | 19-1=18  |
| P<sub>2</sub> | 5   | 38  | 114 | 114-5=109 | 109-38=71 | 76-5=71  |
| P<sub>3</sub> | 6   | 27  | 141 | 141-6=135 | 135-27=108 | 114-6=108 |
| P<sub>4</sub> | 0   | 19  | 19  | 19-0=19 | 19-19=0 | 0-0=0   |
| P<sub>5</sub> | 2   | 35  | 76  | 76-2=74 | 74-35=39 | 41-2=39  |
| P<sub>6</sub> | 18  | 20  | 245 | 245-18=227 | 227-20=207 | 225-18=207 |
| P<sub>7</sub> | 13  | 33  | 225 | 225-13=212 | 212-33=179 | 192-13=179 |
| P<sub>8</sub> | 11  | 39  | 192 | 192-11=181 | 181-39=142 | 153-11=142 |

</center>

### <center>Gant Chart</center>

<center>

| P<sub>4</sub> | P<sub>1</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> | P<sub>0</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>6</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

</center>
<br>

- **Throughput after 130ms = 4**
- **Average Waiting Time =898/9= 99.7**
- **Average Response Time =898/9= 99.78**
- **Average Turnaround Time = 1143/9= 127**

<center>-------------------------------------------------------------------------------------------</center>

## 3. <center>Latest Job First</center>

<center>

| Process | AT  | BT  | CT  | TAT | WT  | RT  |
| --- | --- | --- | --- | --- | --- | --- |
| P<sub>0</sub> | 7   | 12  | 111 | 111-7=104 | 104-12=92 | 0 |
| P<sub>1</sub> | 1   | 22  | 227 | 227-1=226 | 226-22=204 | 0 |
| P<sub>2</sub> | 5   | 38  | 174 | 174-5=169 | 169-38=131 | 0 |
| P<sub>3</sub> | 6   | 27  | 137 | 137-6=131 | 131-27=104 | 0 |
| P<sub>4</sub> | 0   | 19  | 245  | 245-0=245 | 245-19=226 | 0   |
| P<sub>5</sub> | 2   | 35  | 206 | 206-2=204 | 204-35=169 | 0 |
| P<sub>6</sub> | 18  | 20  | 38  | 38-18=20 | 20-20=0 | 0  |
| P<sub>7</sub> | 13  | 33  | 66  | 66-13=53 | 53-33=20 | 0  |
| P<sub>8</sub> | 11  | 39  | 103 | 103-11=92 | 92-39=53 | 0  |

</center>
<br>

### <center>Gant Chart</center>

<center>

| P<sub>4</sub> | P<sub>1</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> | P<sub>0</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>6</sub> | P<sub>7</sub> | P<sub>8</sub> | P<sub>0</sub> | P<sub>3</sub> | P<sub>2</sub> | P<sub>5</sub> | P<sub>1</sub> | P<sub>4</sub> | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

</center>

<br>

- **TAT = 1244/9 = 138.22**
- **Throughput = 4**
- **Average Waiting Time = 999/9 = 111**
- **Average Response Time = 0**
<center>-------------------------------------------------------------------------------------------</center>

## 4. <center>Multilevel Feedback Queue (SJF)</center>

<center>

| Process | AT  | BT  | CT  | TAT | WT  | RT  |
| --- | --- | --- | --- | --- | --- | --- |
| P<sub>0</sub> | 7   | ~~12~~ 6 | 115 | 115-7=108 | 108-12=96 | 23  |
| P<sub>1</sub> | 1   | ~~22 16~~ 5 | 158 | 158-1=157 | 157-22=135 | 5   |
| P<sub>2</sub> | 5   | ~~38 32~~ 21 | 223 | 223-5=218 | 218-38=180 | 13  |
| P<sub>3</sub> | 6   | ~~27 21~~ 10 | 168 | 168-6=162 | 162-27=135 | 18  |
| P<sub>4</sub> | 0   | ~~19 13~~ 2 | 150 | 150-0=150 | 150-19=131 | 0   |
| P<sub>5</sub> | 2   | ~~35 29~~ 18 | 202 | 202-2=200 | 200-35=165 | 10  |
| P<sub>6</sub> | 18 | ~~20 14~~ 13 | 153 | 153-18=135 | 135-20=125 | **30** |
| P<sub>7</sub> | 13  | ~~33 27~~ 16 | 184 | 184-13=171 | 171-33=138 | 29  |
| P<sub>8</sub> | 11  | ~~39 33~~ 22 | 245 | 245-11=234 | 234-39=195 | 25  |

</center>

### <center>Gant Chart</center>

<center>

| P<sub>4</sub> | P<sub>1</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> | P<sub>0</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>6</sub> | P<sub>4</sub> | P<sub>1</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

</center>

<br>

<center>

| P<sub>0</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>6</sub> | P<sub>4</sub> | P<sub>6</sub> | P<sub>1</sub> | P<sub>3</sub> | P<sub>7</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>8</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

</center>

<br>

- **TAT = 1535/9 = 170.56**
- **Throughput = 1**
- **Average Waiting Time = 1290/9 = 143.3**
- **Average Response Time = 153/9 = 17**

<!-- <center>-------------------------------------------------------------------------------------------</center> -->

## 5. <center>Round Robin</center>

<center>

| Process | AT  | BT  | CT  | TAT | WT  | RT  |
| --- | --- | --- | --- | --- | --- | --- |
| P<sub>0</sub> | 7   | **~~12~~ 40** | 116 | 116-7=109 | 109-12=97 | 33  |
| P<sub>1</sub> | 1   | **~~22 14~~ 60** | 141 | 141-1=140 | 140-22=118 | 7   |
| P<sub>2</sub> | 5   | **~~38 30 22 14~~ 6** | 237 | 237-5=232 | 232-38=194 | 19  |
| P<sub>3</sub> | 6   | **~~27 19 11~~ 30** | 112 | 112-6=106 | 106-27=79 | 26  |
| P<sub>4</sub> | 0   | **~~19 11~~ 30** | 119 | 119-0=119 | 119-19=100 | 0   |
| P<sub>5</sub> | 2   | **~~35 27 19 11~~ 30** | 231 | 231-2=229 | 229-35=194 | 14  |
| P<sub>6</sub> | 18  | **~~20 12~~ 40** | 193 | 193-18=175 | 175-20=155 | 62  |
| P<sub>7</sub> | 13  | **~~33 25 17 9~~ 1** | 245 | 245-13=232 | 232-33=199 | 51  |
| P<sub>8</sub> | 11  | **~~39 31 23 15~~ 7** | 244 | 244-11=233 | 233-39=194 | 45  |

</center>

### <center>Ready Queue</center>
P<sub>4</sub> , P<sub>1</sub> , P<sub>5</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>0</sub> , P<sub>4</sub> , P<sub>8</sub> , P<sub>7</sub> , P<sub>1</sub> , P<sub>6</sub> , P<sub>5</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>0</sub> , P<sub>4</sub> , P<sub>8</sub> , P<sub>7</sub> , P<sub>1</sub> , P<sub>6</sub> , P<sub>5</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>8</sub> , P<sub>7</sub> , P<sub>6</sub> , P<sub>5</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>8</sub> , P<sub>7</sub> , P<sub>5</sub> , P<sub>2</sub> , P<sub>8</sub> , P<sub>7</sub>

## <center>Gant Chart</center>

| P<sub>4</sub> | P<sub>1</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> | P<sub>0</sub> | P<sub>4</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>1</sub> | P<sub>6</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
<br>

| P<sub>0</sub> | P<sub>4</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>1</sub> | P<sub>6</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>3</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>6</sub> | P<sub>5</sub> | P<sub>2</sub> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
<br>

<center>

| P<sub>3</sub> | P<sub>8</sub> | P<sub>7</sub> | P<sub>5</sub> | P<sub>2</sub> | P<sub>8</sub> | P<sub>7</sub> |
| --- | --- | --- | --- | --- | --- | --- |

</center>

<br>

- **TAT = 1675/9 = 186.11**
- **Throughput at 130ms = 2**
- **Average Waiting Time = 1430/9 = 158.4**
- **Average Response Time = 257/9 = 28.5**

<center>-------------------------------------------------------------------------------------------</center>

