---
title: "Choosing new open source project to contribute"
date: 2025-04-30 14:00:00 -0300
categories: [Free Software, Contributions]
tags: [kernel, contributions, GNOME, Git, KW]
comments: false
---

After submitting our contribution to the kernel, we moved on to the second phase of the course, which consists of choosing a new open source project to contribute to. Several guest speakers presented very interesting projects, such as Git, KW, and GNOME, and talked about the pros and cons of contributing to each one, their contribution workflows, the culture of interacting with maintainers, and so on. Some technical details were also covered, such as the practice of forking the project before starting to contribute.

## Our choice: GNOME


When analyzing the options, my partner and I were torn between Git and GNOME. However, when we looked at GNOME’s GitLab organization and saw how issues were categorized for newcomers, we felt that this would be the more interesting option. For Git, we had trouble finding good first issues to contribute to.

So, we began the onboarding process for GNOME. We explored the various systems in the project and became interested in GNOME Files, since we found some promising issues already labeled for newcomers. After researching recent issues, this one caught our attention: https://gitlab.gnome.org/GNOME/nautilus/-/issues/3856#note_2433896.

Once we had made our choice, we spoke with our colleagues Otavio and Felipe, who had already contributed to GNOME Calendar. They recommended that we comment on the issues we were interested in, to start engaging with the maintainers and share our ideas for a solution before diving into the code.

That’s when something unexpected happened: after our interaction, the maintainers removed the "newcomer" label from the issue. Still, since we had already started thinking about possible solutions, we decided to continue investigating and try to solve the issue anyway. As a Linux user, it was easy to clone the project on my personal computer and start contributing, especially since the problem reported on Fedora also occurred on Ubuntu (my distro).

Here's a screenshot of the two kernel contributions we are considering:

![Visual Studio Code showing kernel patch options](/assets/img/gnome_gitlab.png)


From there, we began digging into the code, trying to identify the issue and explore potential fixes.