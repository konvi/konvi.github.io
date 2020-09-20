---
layout: post
title:  "Calculate git stats for a team on Windows"
categories: git stats windows
---

What if you need to get added/deleted lines for every member of a team on Windows.

We know how to output amount of modified files for all users or added/deleted lines for a specific user.
{% highlight cmd %}
git shortlog -s
#=> prints amount of modified files and user name to STDOUT.
{% endhighlight %}

{% highlight cmd %}
git log --pretty=tformat: --since="1 Aug, 2020" --until="31 Aug, 2020" --numstat --author="My Name"
#=> prints <added lines> <deleted lines> <File Name> to STDOUT.
{% endhighlight %}

My attempt to do it in bat/cmd/sh wasn't that successful, so here we are: python is going to help. 


{% highlight python %}
import subprocess
import re

leading_4_spaces = re.compile('^    ')

def get_commits():
    fromDate = "1 Aug, 2020"
    toDate = "31 Aug, 2020"
    #git shortlog -s
    lines = subprocess.check_output(
        ['git', 'shortlog', '-s', '--since="1 Aug, 2020"', '--until="31 Aug, 2020"'], stderr=subprocess.STDOUT
    ).decode('utf-8').split('\n')

    names = []
    for line in lines:

        try:
            files, name = line.split(maxsplit=1)
            print(name)
            names.append(name)
        except:
            print("Ooops!")

    print(names)

    for name in names:
        # git log --pretty=tformat: --since="1 Aug, 2020" --until="31 Aug, 2020" --numstat --author="My Name"
        lines = subprocess.check_output(
            ['git', 'log', '--pretty=tformat:', '--since="1 Aug, 2020"', '--until="31 Aug, 2020"', '--numstat', '--author=' + name], stderr=subprocess.STDOUT
        ).decode('utf-8').split('\n')

        total_added = 0
        total_deleted = 0
        for line in lines:
            try:
                #print(line)
                added, deleted, *rest = line.split()
                total_added += int(added)
                total_deleted += int(deleted)
            except ValueError:
                print("Oops!  That was no valid number.  Try again...")

        print(name)
        print(total_added)
        print(total_deleted)

    return

get_commits()
#=> prints <name> <added lines> <deleted lines> to STDOUT.
{% endhighlight %}
