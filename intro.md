# Berkeley Packet Filter (BPF) 
## Introduction
   BPF/eBPF is an in kernel execution engine that processes virtual instruction set.
   
   The technology has grown from a tool that improved performance of packet capture tools [McCanne 92] 
   to a general purpose execution engine that can be used for a variety of things including creation of 
   advanced performance analysis tools.
   
   BPF provides a way to run mini programs on wide variety of kernel and application events.
   An eBPF program is attached to a designated code path in the Kernel.
   When a code path is traversed, any attached eBPF programs are executed.


   BPF is composed of an instruction set, storage objects and helper functions. 
   The main use of BPF are networking, observability and security.
   In this introduction, we will focus on the main frony-end BPF tools used in observability.

1. BPF front-end

    When kernel engineers speak about observability, they refer to tracing, an event-based recording. Tools such as strace and tcpdump can be a good example. BPF provides tools that give extra informations:
	- BCC tools <br/>
	BPF compiler collection (BCC) is the higher level tracing framework developed for BPF.
	The framework provides a C programming environment for writing Kernel BPF code and other languages(python, Lua, C++)  for user-level interface.
BCC repository has more than 70 BPF tools for performance and analysis. We will go through 12 BCC tools.
		- execsnoop<br/>
		This tool works by tracing the execve(2) system call and reveal processes that may be shortlived that they are invisible to other tools like ps. 
		<pre># execsnoop
		PCOMM            PID    PPID   RET ARGS
		dhcpcd-run-hook  29407  2642     0 /lib/dhcpcd/dhcpcd-run-hooks
		sed              29410  29409    0 /bin/sed -n s/^domain //p wlan0.dhcp
		cmp              29417  29407    0 /usr/bin/cmp -s /etc/resolv.conf ../resolv.conf.wlan0.ra
		qemu-system-x86  29422  27546    0 /usr/bin/qemu-system-x86_64 -m 4096 -smp 8 ... -snapshot 
		</pre>
		- opensnoop<br/>
		The tool prints one line of the output per each open() system call with details. 
		opensnoop can be used to troubleshoot failing software which may be attempting to open files from a wrong path as well as determine where the config and log files are kept.
		<pre># opensnoop -T
		TIME(s)       PID    COMM               FD ERR PATH
		0.000000000   11552  baloo_file_extr    20   0 /home/jules/../linux/../unistd_32.h
		0.000433000   11552  baloo_file_extr    20   0 /home/jules/../linux/../unistd_64.h
		0.000764000   11552  baloo_file_extr    20   0 /home/jules/../linux/../unistd_x32.h
		0.001084000   11552  baloo_file_extr    20   0 /home/jules/../linux/../syscalls_32.h
		0.001391000   11552  baloo_file_extr    20   0 /home/jules/../linux/../unistd_32_ia32.h
		0.001685000   11552  baloo_file_extr    20   0 /home/jules/../linux/../unistd_64_x32.h
		0.079771000   3486   qemu-system-x86    23   0 /etc/resolv.conf
		0.422395000   11858  Chrome_IOThread   389   0 /dev/shm/.com.google.Chrome.ct746O </pre>
		- ext4slower
		<pre># ext4slower
                Tracing ext4 operations slower than 10 ms
                TIME     COMM           PID    T BYTES   OFF_KB   LAT(ms) FILENAME
                22:16:08 baloo_file_ext 4458   S 0       0         125.20 index
                22:16:12 baloo_file_ext 4458   S 0       0         134.65 index
                22:16:16 baloo_file_ext 4458   S 0       0         151.65 index
                22:16:20 baloo_file_ext 4458   S 0       0         172.81 index
                22:16:25 baloo_file_ext 4458   W 60678144 5098540    11.48 index
		</pre>
		- biolatency 
		<pre># biolatency
                Tracing block device I/O... Hit Ctrl-C to end.
                ^C
                usecs               : count     distribution
                0 -> 1          : 0        |                                        |
                2 -> 3          : 0        |                                        |
                4 -> 7          : 3        |                                        |
                8 -> 15         : 115      |**************                          |
                16 -> 31         : 49       |******                                  |
                32 -> 63         : 36       |****                                    |
                64 -> 127        : 1        |                                        |
                128 -> 255        : 286      |************************************    |
                256 -> 511        : 160      |********************                    |
                512 -> 1023       : 315      |****************************************|
                1024 -> 2047       : 21       |**                                      |
                2048 -> 4095       : 1        |                                        |

		</pre>
		- biosnoop
		<pre># biosnoop
		TIME(s)     COMM           PID    DISK    T SECTOR     BYTES  LAT(ms)
		0.000000    kworker/23:1   9126           R 18446744073709551615 0         0.61
		1.774198    ThreadPoolFore 5270   nvme0n1 W 520198144  225280    0.48
		1.774381    jbd2/nvme0n1p3 686    nvme0n1 W 490161296  65536     0.03
		1.774609    ?              0              R 0          0         0.21
		1.774809    jbd2/nvme0n1p3 686    nvme0n1 W 490161424  4096      0.19
		2.069546    kworker/23:1   9126           R 18446744073709551615 0         0.17
		2.159061    ?              0              R 0          0         0.24
		2.159129    ThreadPoolFore 5270   nvme0n1 W 777702184  4096      0.01
		2.159341    ?              0              R 0          0         0.20
		2.159387    ThreadPoolFore 5270   nvme0n1 W 15221256   8192      0.01
		2.159598    ?              0              R 0          0         0.20
		2.159713    jbd2/nvme0n1p3 686    nvme0n1 W 490161432  53248     0.02</pre>


bpftrace

  Front end that provides a special purpose high level lanbguage for developing BPF tools
