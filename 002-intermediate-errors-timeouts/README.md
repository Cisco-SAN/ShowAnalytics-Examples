# An intermediate level example to display additional errors via  ShowAnalytics utility
This example guides you to display additional error counters by the ShowAnalytics utility.

Before (using the default ShowAnalytics utility from NX-OS 8.4(1): [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py)):

   

    MDS9718-A# ShowAnalytics --errors --target-itl --limit 5
    2020-03-13 10:16:39.646573
    
     Interface fc1/33
    +------------------------------------------+---------------------+-----------------+
    |        VSAN|Initiator|Target|LUN         | Total SCSI Failures | Total FC Aborts |
    +------------------------------------------+---------------------+-----------------+
    |                                          |     Read | Write    |   Read | Write  |
    |                                          |                     |                 |
    | 20|0x320074|0x050021|000e-0000-0000-0000 |        0 | 0        |      0 | 0      |
    | 20|0x320074|0x050021|001e-0000-0000-0000 |        0 | 0        |      0 | 0      |
    | 20|0x320073|0x050021|0005-0000-0000-0000 |        0 | 0        |      0 | 0      |
    | 20|0x320074|0x050021|0014-0000-0000-0000 |        0 | 0        |      0 | 0      |
    | 20|0x320072|0x050021|001b-0000-0000-0000 |        0 | 0        |      0 | 0      |
    +------------------------------------------+---------------------+-----------------+


After (using [analytics-errors-timeouts.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/002-intermediate-errors-timeouts/analytics-errors-timeouts.py)):

    MDS9718-A# python bootflash:///scripts/analytics-errors-timeouts.py --errors --target-itl --limit 5
    2020-03-13 10:57:21.072663
    
     Interface fc1/33
    +------------------------------------------+---------------------+-----------------+----------------+
    |        VSAN|Initiator|Target|LUN         | Total SCSI Failures | Total FC Aborts | Total Timeouts |
    +------------------------------------------+---------------------+-----------------+----------------+
    |                                          |     Read | Write    |   Read | Write  |  Read | Write  |
    |                                          |                     |                 |                |
    | 20|0x320074|0x050021|000e-0000-0000-0000 |        0 | 0        |      0 | 0      |   212 | 274    |
    | 20|0x320074|0x050021|001e-0000-0000-0000 |        0 | 0        |      0 | 0      |   147 | 235    |
    | 20|0x320073|0x050021|0005-0000-0000-0000 |        0 | 0        |      0 | 0      |   83  | 133    |
    | 20|0x320074|0x050021|0014-0000-0000-0000 |        0 | 0        |      0 | 0      |   217 | 270    |
    | 20|0x320072|0x050021|001b-0000-0000-0000 |        0 | 0        |      0 | 0      |   89  | 139    |
    +------------------------------------------+---------------------+-----------------+----------------+
Notice a 3rd column displaying Timeouts.
 

The overall workflow should be:
 1. Export the original ShowAnalytics utility [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py) from MDS switch to your development workstation/laptop.
 2. Edit [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py) using your preferred text editor. Optionally, save the changed file as [analytics-errors-timeouts.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/002-intermediate-errors-timeouts/analytics-errors-timeouts.py) to avoid any confusion later. This example does this.
 3. Copy the changed file back to the MDS switch.
 4. Verify the changes.

Please look at the diffs for the exact changed. In brief:

 1. You need to change the SQL-like queries to pull read_io_timeouts and write_io_timeouts from the NPU Database.
 2. Modify displayErrorsOverlay() function to display the counters. *Hint: Follow the sequence for read_io_aborts.*

