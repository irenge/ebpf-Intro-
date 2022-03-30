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
		- execsnoop - works by tracing the execve(2) system call and reveal processes that may be shortlived that they are invisible to other tools like ps. 
		`jules # execsnoop
PCOMM            PID    PPID   RET ARGS
dhcpcd-run-hook  29407  2642     0 /lib/dhcpcd/dhcpcd-run-hooks
sed              29410  29409    0 /bin/sed -n s/^domain //p wlan0.dhcp
sed              29412  29411    0 /bin/sed -n s/^search //p wlan0.dhcp
sed              29415  29414    0 /bin/sed -n s/^nameserver //p wlan0.dhcp
cmp              29417  29407    0 /usr/bin/cmp -s /etc/resolv.conf /run/dhcpcd/hook-state/resolv.conf.wlan0.ra
rm               29418  29407    0 /bin/rm -f /run/dhcpcd/hook-state/resolv.conf.wlan0.ra
rm               29419  29407    0 /bin/rm -f /run/dhcpcd/hook-state/resolv.conf.wlan0.ra
qemu-system-x86  29422  27546    0 /usr/bin/qemu-system-x86_64 -m 4096 -smp 8 -net nic,model=e1000 -net user,host=10.0.2.10,hostfwd=tcp::45506-:22 -display none -serial stdio -no-reboot -enable-kvm -cpu host,migratable=off -hda /home/jules/work/report/syzVegas/image/stretch.img -snapshot
ssh              29445  27546    0 /usr/bin/ssh -p 45506 -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa root@localhost pwd
scp              29447  27546    0 /usr/bin/scp -P 45506 -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa /home/jules/work/report/syzVegas/gopath/src/github.com/google/syzkaller/bin/linux_amd64/syz-fuzzer root@localhost:/syz-fuzzer
ssh              29448  29447    0 /usr/bin/ssh -x -oPermitLocalCommand=no -oClearAllForwardings=yes -oRemoteCommand=none -oRequestTTY=no -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa
scp              29449  27546    0 /usr/bin/scp -P 45506 -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa /home/jules/work/report/syzVegas/gopath/src/github.com/google/syzkaller/bin/linux_amd64/syz-executor root@localhost:/syz-executor
ssh              29450  29449    0 /usr/bin/ssh -x -oPermitLocalCommand=no -oClearAllForwardings=yes -oRemoteCommand=none -oRequestTTY=no -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa
ssh              29451  27546    0 /usr/bin/ssh -p 45506 -F /dev/null -o UserKnownHostsFile=/dev/null -o BatchMode=yes -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o ConnectTimeout=10 -i /home/jules/work/report/syzVegas/image/stretch.id_rsa root@localhost cd / && /syz-fuzzer -executor=/syz-executor -name=vm-1 -arch=amd64 -manager=10.0.2.10:36111 -sandbox=none -procs=8 -cover=true -`
	

bpftrace
  Front end that provides a special purpose high level lanbguage for developing BPF tools
