## Work done during mentorship
<ol>
<li> Accepted patches in the Linux Kernel</li>

During this mentorship, few patches were accepted and merges into the Linux Kernel.  As my profile can show [here](https://linuxlists.cc/profile/52567/Jules_Irenge), I submitted more than 30 patches.

Some of the patches have been merged to the Linux Kernel [1] (https://lore.kernel.org/all/Yxi3pJaK6UDjVJSy@playground/)

All accepted patches can be found on [2] (https://lore.kernel.org/all/?q=Jules+Irenge)

Practice makes better, working in Kernel is no exception. The more I sent patches the better I become at it.   One patch from which I learnt a lot is one that I made a change that appear simple in the BPF subsystem  but ended up being a bug fix [3](https://git.kernel.org/pub/scm/linux/kernel/git/bpf/bpf-next.git/commit/?id=9fad7fe5b298).<br/>The interaction I had with the BPF community in the mailing list was particularly welcoming, fun and learning experience.

<li> Attempted but not accepted </li>
On the request of Daniel Borkmann, I attempted to migrating the sample code [0]
from BPF over to the BPF selftests [1]:
   [0] https://git.kernel.org/pub/scm/linux/kernel/git/bpf/bpf-next.git/tree/samples/bpf
   [1] https://git.kernel.org/pub/scm/linux/kernel/git/bpf/bpf-next.git/tree/tools/testing/selftests/bpf

The objective was to reduce the mess as some few tests that are in the samples directory are not run on BPF CI
Combining the file into one will increase the test on BPF CI and increase reliability on BPF subsystem.

  - https://git.kernel.org/pub/scm/linux/kernel/git/bpf/bpf-next.git/tree/tools/testing/selftests/bpf/prog_tests
  - https://git.kernel.org/pub/scm/linux/kernel/git/bpf/bpf-next.git/tree/tools/testing/selftests/bpf/progs

But the long patch was not accepted, I suspect it was because it features a lot of changes.

<li> Ongoing </li>
<ul>
<li> xdp tools </li>

I spent time reading about xdp and practicing. I ended up building a tool to count how many packets have been transmitted, passed and dropped
the tool present a percentage of dropped packet.






On my studies of the BPF subsystem
 <li> bcc tool issue assigned </li> 
</ul>
<li> Future task </li>
</ol>

