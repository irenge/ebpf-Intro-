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
		- execsnoop
		This tool works by tracing the execve(2) system call and reveal processes that may be shortlived that they are invisible to other tools like ps. 
		<pre># execsnoop
		PCOMM            PID    PPID   RET ARGS
		dhcpcd-run-hook  29407  2642     0 /lib/dhcpcd/dhcpcd-run-hooks
		sed              29410  29409    0 /bin/sed -n s/^domain //p wlan0.dhcp
		cmp              29417  29407    0 /usr/bin/cmp -s /etc/resolv.conf ../resolv.conf.wlan0.ra
		qemu-system-x86  29422  27546    0 /usr/bin/qemu-system-x86_64 -m 4096 -smp 8 ... -snapshot</pre>

bpftrace
  Front end that provides a special purpose high level lanbguage for developing BPF tools
