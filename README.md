
# ShowAnalytics - The first programmable command on Cisco MDS 9000 switches

ShowAnalytics is the first programmable command on Cisco MDS 9000 switches which helps you unleash the power of Cisco SAN Analytics.

## Background - Cisco SAN Analytics

Cisco SAN Analytics provides IO performance metrics natively on the Cisco MDS 9000 switches. As IO transactions (read and write commands) are exchanged between host and storage, the Cisco MDS switches perform an in-line inspection of the frame headers. This is made possible by the ASIC integrated TAPs and an on-board Network Processing Unit (NPU).

The IO flow metrics are accumulated in an in-memory, hierarchical and relations database. You can use raw SQL-like commands to access metrics from the NPU database. The output of the SQL-like commands is in JSON format which is great for machine parsing but not convenient for human eyes.

## The ShowAnalytics command

The ShowAnalytics command is an overlay Python utility on top of the raw SQL-like queries. You can use it for real-time visibility and troubleshooting on the Cisco MDS 9000 switches. Most importantly, you can change the data output and format of this utility. Essentially, the ShowAnalytics command is an alias to a Python utility which is invoked using the on-switch Python interpreter. While Cisco has been enhancing this utility with every release, you can edit the backend file to solve your own use-cases within minutes or hours. You do not have to wait for Cisco to enhance this command or you to upgrade the firmware on the switches.

## The purpose of this repository

This repository provides examples to enhance the ShowAnalytics utility.

**If you code**: Please contribute and share.

**If you don't code**: Feel free to make feature requests here.

**If you want to learn**: This is the place for you

After following the first 4 examples, you will be ready to enhance the ShowAnalytics utility.

## Pre-requsite

You need a basic experience of working with Python. If this is not your thing, try some beginner tutorials on YouTube.

You don't need to be an expert programmer. The base code exists. You can follow the existing flow to implement your own logic.

## Help
Do a web search: Cisco MDS san analytics config guide. 
The official documentation has details on the SQL-like raw queries which you need to use to pull the metrics from the NPU DB. The official documentation also covers read-made use-cases shipped within NX-OS.

## Notes

1. With great power comes great responsibility. The on-board Python interpreter gives you limit-less options. Do not abuse it. Try not to run infinite for-loop to bring down the switch.

2. Avoid running heavy computations on the switch. Even though CPUs on the latest generation of the switches are powerful, the capacity is still finite. Heavy computations at scale are best implemented on a remote machine using the streaming telemetry.

3. This should be obvious, but Cisco's official support is limited to the ShowAnalytics utility shipped with NX-OS. If the ShowAnalytics utility does not work after your changes, you won't be able to fil a bug with Cisco. You are welcome to open issues on GitHub and someone may try to help you but this is going to be on best-effort basis only. Having said that, this is not rocket science. You can do it.
