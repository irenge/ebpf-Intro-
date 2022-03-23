# Berkeley Packet Filter (BPF)  
   Originally improved the performance of packet capture tools.
   BPF provides a way to run mini programs on wide variety of kernel and application events.
   It allows the kernel to run mini program on system and application events such as disk I/O
   
   eBPF stands for extended Berkeley Packet Filter 

The main use of BPF are networking, observability and security. 

1. Observability

    Tracing  is event-based recording
     eg. strace
     tcpdump 


BCC 
  BPF compiler collection (BCC) is a high level tracing framework developed for BPF.
  The framework provides a C programming environment for writing Kernel BPF code and other languages(python, Lua, C++)  for user-level interface.

bpftrace
  Front end that provides a special purpose high level lanbguage for developing BPF tools
