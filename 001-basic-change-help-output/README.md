# Basic example to change ShowAnalytics utility
If you are trying to make an initial attempt to change the ShowAnalytics utility, this is a basic example to explain the overall workflow. To keep it simple, only the help output is changed. No functional changes are made in this example.

The overall workflow should be:
 1. Export the original ShowAnalytics utility [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py) from MDS switch to your development workstation/laptop.
 2. Edit [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py) using your preferred text editor. Optionally, save the changed file as [analytics-change-help-output.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/basic_1_change_help_output/analytics-change-help-output.py) to avoid any confusion later. This example does this.
 3. Copy the changed file back to the MDS switch.
 4. Verify the changes.

Before (using the default ShowAnalytics utility from NX-OS 8.4(1): [analytics.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/analytics.py)):

    MDS9718-A# ShowAnalytics -h
    
    ShowAnalytics   --errors <options> | --errorsonly <options> | --evaluate-npuload <options> | --help | --info <options> | --minmax <options> | --outstanding-io <options> | --top <options> | --version |  --vsan-thput <options>


After (using [analytics-change-help-output.py](https://github.com/Cisco-SAN/ShowAnalytics-Examples/blob/master/basic_1_change_help_output/analytics-change-help-output.py)):

    MDS9718-A# python bootflash:///scripts/analytics-change-help-output.py -h
    
        --- Example 1 - Changed output - Success ---
    
    ShowAnalytics   --errors <options> | --errorsonly <options> | --evaluate-npuload <options> | --help | --info <options> | --minmax <options> | --outstanding-io <options> | --top <options> | --version |  --vsan-thput <options>
    <snip>

Notice the additional text:

        --- Example 1 - Changed output - Success ---

Alternatively, you can create an alias for the above command. However, there is no functional difference between the two approach. The below approach just a *nicer* way.

    MDS9718-A# conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    MDS9718-A(config)# cli alias name ShowAnalyticsExample1 source analytics-change-help-output.py
    MDS9718-A(config)# end
    MDS9718-A# ShowAnalyticsExample1 -h
    
        --- Example 1 - Changed output - Success ---
    
    ShowAnalytics   --errors <options> | --errorsonly <options> | --evaluate-npuload <options> | --help | --info <options> | --minmax <options> | --outstanding-io <options> | --top <options> | --version |  --vsan-thput <options>
