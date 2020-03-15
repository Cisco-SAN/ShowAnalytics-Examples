# An intermediate level example to display (%) DAL with ECT using --top option of ShowAnalytics utility
The ShowAnalytics utility shipped with NX-OS 8.4(1) provides an option to display top 10 ITL/ITN flows with highest ECT values. This example adds two new columns to display DAL and % DAL of ECT. Wondering what is the use-case? Read the explanation in the Background section.

Before (using the default ShowAnalytics utility from NX-OS 8.4(1): [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py)):

    MDS9718-A# ShowAnalytics --top --key ECT
    
    2020-03-14 20:36:26.068384
    
    +--------+------------------------------------------+----------------------+
    |  PORT  |        VSAN|Initiator|Target|LUN         |         ECT          |
    +--------+------------------------------------------+----------------------+
    |        |                                          |    Read  |  Write    |
    | fc1/35 | 20|0x320066|0x050101|0025-0000-0000-0000 |  13.5 ms | 117.4 ms  |
    | fc1/33 | 20|0x320076|0x050021|001c-0000-0000-0000 |  15.8 ms | 114.3 ms  |
    | fc1/33 | 20|0x320076|0x050021|0016-0000-0000-0000 |  13.2 ms | 114.1 ms  |
    | fc1/34 | 20|0x320076|0x050041|0003-0000-0000-0000 |  16.1 ms | 109.4 ms  |
    | fc1/33 | 20|0x320076|0x050021|0029-0000-0000-0000 |   8.9 ms | 115.3 ms  |
    | fc1/34 | 20|0x320076|0x050041|002a-0000-0000-0000 |  11.9 ms | 111.2 ms  |
    | fc1/36 | 20|0x320061|0x050121|0019-0000-0000-0000 |  19.3 ms | 103.4 ms  |
    | fc1/35 | 20|0x320076|0x050101|0029-0000-0000-0000 |  12.6 ms | 109.7 ms  |
    | fc1/34 | 20|0x320064|0x050041|001b-0000-0000-0000 |  26.0 ms | 96.2 ms   |
    | fc1/33 | 20|0x320067|0x050021|001b-0000-0000-0000 |  10.7 ms | 111.4 ms  |
    +--------+------------------------------------------+----------------------+

After (using [analytics-top-ect-with-dal.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/003-intermediate-top-ect-with-dal/analytics-top-ect-with-dal.py)):

    MDS9718-A# python bootflash:///scripts/analytics-top-ect-with-dal.py --top --key ECT
    
    2020-03-14 20:39:18.315232
    
    +--------+------------------------------------------+----------------------+----------------------+
    |  PORT  |        VSAN|Initiator|Target|LUN         |         ECT          |         DAL          |
    +--------+------------------------------------------+----------------------+----------------------+
    |        |                                          |    Read  |  Write    |    Read  |  Write    |
    | fc1/34 | 20|0x320076|0x050041|0029-0000-0000-0000 |  29.6 ms | 50.4 ms   |  29.6 ms | 33.0 us   |
    | fc1/35 | 20|0x320076|0x050101|000e-0000-0000-0000 |  26.9 ms | 50.6 ms   |  26.9 ms | 44.0 us   |
    | fc1/34 | 20|0x320076|0x050041|000b-0000-0000-0000 |  27.6 ms | 49.2 ms   |  27.6 ms | 43.0 us   |
    | fc1/35 | 20|0x320066|0x050101|0027-0000-0000-0000 |  29.0 ms | 46.6 ms   |  29.0 ms | 51.0 us   |
    | fc1/34 | 20|0x320076|0x050041|001c-0000-0000-0000 |  29.6 ms | 45.9 ms   |  29.5 ms | 182.0 us  |
    | fc1/34 | 20|0x320076|0x050041|0009-0000-0000-0000 |  29.8 ms | 45.6 ms   |  29.8 ms | 52.0 us   |
    | fc1/34 | 20|0x320067|0x050041|001c-0000-0000-0000 |  29.4 ms | 45.0 ms   |  29.4 ms | 69.0 us   |
    | fc1/35 | 20|0x320066|0x050101|0026-0000-0000-0000 |  28.7 ms | 45.4 ms   |  28.7 ms | 95.0 us   |
    | fc1/36 | 20|0x320061|0x050121|000b-0000-0000-0000 |  29.8 ms | 44.1 ms   |  29.8 ms | 58.0 us   |
    | fc1/34 | 20|0x320067|0x050041|0023-0000-0000-0000 |  29.3 ms | 44.6 ms   |  29.3 ms | 122.0 us  |
    +--------+------------------------------------------+----------------------+----------------------+

Notice a new column displaying the DAL values. But maual comparison between ECT and DAL is not intuitive. To make it simple, you can print another column displaying the %DAL of to the overall ECT values.

After another change (using [analytics-top-ect-with-dal-percent.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/003-intermediate-top-ect-with-dal/analytics-top-ect-with-dal-percent.py)):

    MDS9718-A# python bootflash:///scripts/analytics-top-ect-with-dal-percent.py --top --key ECT
    
    2020-03-14 20:44:27.966821
    
    +--------+------------------------------------------+----------------------+----------------------+----------------------+
    |  PORT  |        VSAN|Initiator|Target|LUN         |         ECT          |         DAL          |      % DAL/ECT       |
    +--------+------------------------------------------+----------------------+----------------------+----------------------+
    |        |                                          |    Read  |  Write    |    Read  |  Write    |    Read  |  Write    |
    | fc1/34 | 20|0x320061|0x050041|000b-0000-0000-0000 |  25.7 ms | 128.8 ms  |  25.7 ms | 366.0 us  |   99.96% |  0.28%    |
    | fc1/34 | 20|0x320076|0x050041|000e-0000-0000-0000 |  19.6 ms | 131.3 ms  |  19.6 ms | 46.0 us   |   99.95% |  0.04%    |
    | fc1/35 | 20|0x320061|0x050101|0026-0000-0000-0000 |  29.7 ms | 106.9 ms  |  29.7 ms | 73.0 us   |   99.95% |  0.07%    |
    | fc1/35 | 20|0x320066|0x050101|001c-0000-0000-0000 |  21.4 ms | 114.6 ms  |  21.4 ms | 45.0 us   |   99.95% |  0.04%    |
    | fc1/34 | 20|0x320076|0x050041|0029-0000-0000-0000 |  21.7 ms | 113.4 ms  |  21.7 ms | 45.0 us   |   99.96% |  0.04%    |
    | fc1/36 | 20|0x320076|0x050121|0029-0000-0000-0000 |  26.6 ms | 104.7 ms  |  26.6 ms | 35.0 us   |   99.97% |  0.03%    |
    | fc1/35 | 20|0x320076|0x050101|0004-0000-0000-0000 |  24.4 ms | 106.6 ms  |  24.4 ms | 310.0 us  |   99.96% |  0.29%    |
    | fc1/34 | 20|0x320066|0x050041|002f-0000-0000-0000 |  22.9 ms | 106.5 ms  |  22.9 ms | 247.0 us  |   99.96% |  0.23%    |
    | fc1/34 | 20|0x320076|0x050041|000a-0000-0000-0000 |  23.4 ms | 104.5 ms  |  23.4 ms | 34.0 us   |   99.96% |  0.03%    |
    | fc1/36 | 20|0x320076|0x050121|001d-0000-0000-0000 |  26.7 ms | 100.2 ms  |  26.7 ms | 77.0 us   |   99.97% |  0.08%    |
    +--------+------------------------------------------+----------------------+----------------------+----------------------+

 Notice another colum with title '%DAL/ECT'.
 
Please refer to the diffs for the exact changes. In brief:

 1. Change the SQL-like queries to pull total_read_io_initiation_time and total_write_io_initiation_time from the NPU Database.
 2. Modify displayTop() function to display the counters. *Hint: Follow the sequence for ECT.*

## Background
ECT (AKA Exchange completion time or IO completion time) is the total time taken to complete a read or write IO operation. Higher ECT values are not generally good for application performance. Notice that the above output shows read ECT in 10s of ms and write ECT in 100s of ms. Such high values for All Flash Arrays is a matter of concern. Once you know the high values of ECT, your next step would be to find why the ECT values are higher. For this you need to understand the read and write operations. Please refer to the below image. The time taken for a storage array to send first response to a read or write command is called Data Access Latency or DAL. An increase in DAL also increases ECT. If this happens, you can suspect a slowdown within storage array. In the above outputs, notice that DAL is causing more than 99% delay in the overall ECT values for read operations.

But ECT can still increase without any increase in DAL. If this happens, you can rule out any issues with storage arrays and focus your troubleshooting within the SAN or host. In the above output, notice that DAL is causing less than 1% delay in the overall ECT values for write operations.

![](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/003-intermediate-top-ect-with-dal/scsi_read_write_cmd.jpg)
