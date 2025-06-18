---
title: "Results from Our Contribution to the Linux Kernel and GNOME Files"
date: 2025-05-31 14:00:00 -0300
categories: [Linux Kernel, GNOME]
comments: false
---

After a few days of waiting, we finally received responses regarding our contributions to the Linux Kernel and the GNOME Files system.

## Results on the Kernel

At first, we were a little bit worried because several days had passed without any response. It was funny that even another maintainer had to reply to the thread asking for a follow-up. Twice :)

![Visual Studio Code showing kernel patch options](/assets/img/ask_answer_kernel.png)

Even though the reply from the main maintainer took a while, it was worth the wait — he liked our code and accepted our contribution! Yay!

![Visual Studio Code showing kernel patch options](/assets/img/accepting_kernel.png)

Finally, we received confirmation that our patch would be applied:

![Visual Studio Code showing kernel patch options](/assets/img/apply_kernel.png)

## Results on GNOME Files

On the other hand, the news wasn’t so great with GNOME.

As explained in a previous post, the maintainer had made a mistaken assumption about the issue. Still, we were intrigued and decided to try solving the problem anyway.

After a lot of work (this issue turned out to be way more complex than the Kernel one), we actually solved it — as shown in a previous post. We successfully made the bit rate display correctly. The problem was that the maintainer believed it wasn’t a good idea to perform the calculation manually as we had done, since the library used (GStreamer) should ideally handle it.

![Visual Studio Code showing kernel patch options](/assets/img/reply1_gnome.png)

Since GStreamer should take care of the calculation, there was really nothing left for us to do. So, we decided to look for other issues, and also considered moving to another free software project: Git — a personal goal of mine, which you'll read more about in future posts :-)

Even though our contribution was rejected, the maintainer was really kind, trying not to let us get discouraged by the rejection.

![Visual Studio Code showing kernel patch options](/assets/img/final_no_gnome.jpeg)

## Conclusions

I’d like to say that the experience in both projects was really rewarding. In the Kernel project, beyond having our contribution accepted, we got the whole package a newcomer could ask for. We learned more about the kernel, how the contribution workflow works, got our hands dirty, and had great interactions with the maintainers — who not only asked us to make changes but also explained *why*. It wasn’t just mechanical; we truly understood what was going on.

The same happened in GNOME, where the maintainer was welcoming and showed that the community is excited to support and encourage new people to dive into the free software world.