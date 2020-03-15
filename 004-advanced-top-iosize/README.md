# An advanced level example to display ITL/ITN flows with top Read and Write IO sizes
The ShowAnalytics utility shipped with NX-OS 8.4(1) provides an option to display top 10 ITL/ITN flows with highest IOPS, Throughput and ECT values. This example adds two new options: RIOSIZE and WIOSIZE.

Before (using the default ShowAnalytics utility from NX-OS 8.4(1): [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py)):

    MDS9718-A# ShowAnalytics --help
    <snip>
     --top                    Provides top ITLs based on key. Default key is IOPS
                              Args : [--interface <interface>] [--initiator <initiator_fcid>] [--target <target_fcid>] [--lun <lun_id>] [--limit] [--key <IOPS|THPUT|ECT>] [--progress]
    <snip>

After (using [analytics-top-iosize.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/004-advanced-top-iosize/analytics-top-iosize.py)):

    MDS9718-A# python bootflash:///scripts/analytics-top-iosize.py --help
    <snip>
     --top                    Provides top ITLs based on key. Default key is IOPS
                              Args : [--interface <interface>] [--initiator <initiator_fcid>] [--target <target_fcid>] [--lun <lun_id>] [--limit] [--key <IOPS|THPUT|ECT|RIOSIZE|WIOSIZE>] [--progress]
    <snip>
    MDS9718-A# python bootflash:///scripts/analytics-top-iosize.py --top --key RIOSIZE
    
    2020-03-14 22:23:34.121505
    
    +--------+------------------------------------------+-------------------+
    |  PORT  |        VSAN|Initiator|Target|LUN         |      IO SIZE      |
    +--------+------------------------------------------+-------------------+
    |        |                                          |   Read  |  Write  |
    | fc1/35 | 20|0x320076|0x050101|002c-0000-0000-0000 |  1.2 MB |32.0 KB  |
    | fc1/34 | 20|0x320076|0x050041|000c-0000-0000-0000 |  1.1 MB |32.0 KB  |
    | fc1/33 | 20|0x320076|0x050021|002f-0000-0000-0000 |  1.0 MB |25.6 KB  |
    | fc1/35 | 20|0x320076|0x050101|001b-0000-0000-0000 |  1.0 MB |48.0 KB  |
    | fc1/33 | 20|0x320076|0x050021|0017-0000-0000-0000 | 992.0 KB|27.4 KB  |
    | fc1/33 | 20|0x320076|0x050021|0026-0000-0000-0000 | 992.0 KB|32.0 KB  |
    | fc1/33 | 20|0x320076|0x050021|0022-0000-0000-0000 | 960.0 KB|32.0 KB  |
    | fc1/34 | 20|0x320076|0x050041|0025-0000-0000-0000 | 960.0 KB|28.0 KB  |
    | fc1/35 | 20|0x320076|0x050101|001a-0000-0000-0000 | 960.0 KB|32.0 KB  |
    | fc1/34 | 20|0x320076|0x050041|0014-0000-0000-0000 | 928.0 KB|32.0 KB  |
    +--------+------------------------------------------+-------------------+
    
    ^C
    Received ctrl + c
    MDS9718-A# python bootflash:///scripts/analytics-top-iosize.py --top --key WIOSIZE
    
    2020-03-14 22:23:48.785010
    
    +--------+------------------------------------------+-------------------+
    |  PORT  |        VSAN|Initiator|Target|LUN         |      IO SIZE      |
    +--------+------------------------------------------+-------------------+
    |        |                                          |   Read  |  Write  |
    | fc1/35 | 20|0x320061|0x050101|000f-0000-0000-0000 | 106.7 KB|54.7 KB  |
    | fc1/34 | 20|0x320061|0x050041|002c-0000-0000-0000 | 352.0 KB|53.3 KB  |
    | fc1/35 | 20|0x320067|0x050101|0018-0000-0000-0000 | 192.0 KB|53.3 KB  |
    | fc1/33 | 20|0x320066|0x050021|0016-0000-0000-0000 | 256.0 KB|51.2 KB  |
    | fc1/34 | 20|0x320067|0x050041|0023-0000-0000-0000 | 256.0 KB|51.2 KB  |
    | fc1/34 | 20|0x320060|0x050041|0007-0000-0000-0000 | 80.0 KB |51.2 KB  |
    | fc1/33 | 20|0x320061|0x050021|0012-0000-0000-0000 | 96.0 KB |48.0 KB  |
    | fc1/33 | 20|0x320061|0x050021|001b-0000-0000-0000 | 80.0 KB |48.0 KB  |
    | fc1/33 | 20|0x320076|0x050021|001c-0000-0000-0000 | 119.3 KB|48.0 KB  |
    | fc1/33 | 20|0x320066|0x050021|0030-0000-0000-0000 | 80.0 KB |48.0 KB  |
    +--------+------------------------------------------+-------------------+
    
    ^C
    Received ctrl + c
    MDS9718-A#

Notice the new options with --top --key and also the output of ITL flows with top Read IO sizes and Write IO Sizes.
 
Please refer to the diffs for the exact changes. In brief:

 1. Change the SQL-like queries to pull total_read_io_bytes and total_write_io_bytes from the NPU Database.
 2. Modify displayTop() function to display the counters. *Hint: Follow the sequence for ECT.*
 3. Add new input parameters
 4. Add a new function iosize_conv(), to convert IO sizes into MB, KB, B, etc.
