---
layout: page
title: pomodoro system design
description: establishing a more productive, balanced lifestyle
img: assets/img/2205_books/book_collage_v2.png 
importance: 1
category: growth
---

<br>
# Overview
Years ago I was frustrated with how I worked, suffering from undirected time at my desk and undefined boundaries between work and personal time. I was far from the efficient worker and human I wanted to be. 

Initially a quest to become more focused, this project evolved to completely transform my orientation toward work. The result has been higher productivity and a more balanced life. Hopefully in reading this you can find a nugget of insight for your own journey.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% responsive_image path: assets/img/2205_books/book_collage_v2.png title: "book collage" class: "img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
<a href="https://mytomatoes.com/">Pomodoro tracker</a> which allows recording of how each 25-minute interval is used.
</div>

<br>
# Pomodoro Method
The [pomodoro method](https://en.wikipedia.org/wiki/Pomodoro_Technique) is a time-management technique which separates work into fixed intervals, referred to as "tomatoes" (pomodoro is Italian for tomato). Typical length for one tomato is 25 minutes. Critically, this time must be committed to pure work; if one becomes distracted or interrupted, the rule is to "squash" that tomato and re-start the clock. Personally, turning off notifications has been indispensable for avoiding squashes.

To track tomatoes, I use [this simple web tool](https://mytomatoes.com/), which breaks one interval into four steps: (1) start the timer (2) work for 25 minutes (3) record what you did (4) break for 5 minutes. I modify this to skip the breaks, opting for 4-8 consecutive tomatoes before taking a longer rest.

There are many pomodoro trackers out there, but I really like this one prompting "what did you do?" It forces me to pop my head up and briefly evaluate how my current sub-task contributes toward the overall project objective. Many unproductive rabbit holes have been avoided this way. 

Another unexpected benefit is that starting a tomato can help me overcome the activation energy for beginning work. Say I'm feeling tired, or there's a task I just don't feel like doing. I'll negotiate with myself: "spend 1-2 tomatoes on this task; if afterward you're still not in the mood, you can stop and do something else." Usually after a few minutes, I find myself in the groove and motivated to continue working.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% responsive_image path: assets/img/2205_books/book_collage_v2.png title: "book collage" class: "img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    My responses to "what did you do?" this past year, depicting how my time is used. To create your own word cloud, here's my <a href="xxx">python code</a>.
</div>

<br>
# System 1: Daily Quota
Initially I employed a simple system: each day, accomplish $$\geq n$$ tomatoes. This number may be tuned depending on the week, but typically $$n=16$$. Essential meetings count as one tomato; this incentivizes shorter, efficient meetings and still makes the quota attainable on days my calendar is jammed. 

Overall this system helped me develop a focus mindset, and most days I felt empowered. However, it left room for improvement in these two scenarios:
* Not reaching $$n$$. Some days things come up, and life pulls in a different direction. I can't control external circumstances; that's okay––I just want a system which reflects that.
* Far exceeding $$n$$. The nature of my work is that I can always do more; knowing when to stop is difficult, and sometimes I'd end up doing 25+ tomatoes. While initially the productivity felt great, later in the week I might crash and feel exhausted. In addition to making me inconsistent, these energy deficits detracted from my mood, motivation, and personal life.

<br>
# System 2: Weekly Quota + Daily Cap
Instead of a lower bound for the day, I now set one for the week ($$\geq 5n$$ weekly) in addition to an upper bound each day ($$ \leq n$$ daily). That is, once I achieve $$n$$, I must stop working that day. This new success metric has made a huge difference:
* _Motivates an early start_. The sooner I begin, the sooner I have free time. Also I feel more incentivized to zone in for small bursts, e.g. given 30 minutes between meetings.
* _Spreads responsibility across the week_. It's easier to accept lighter work days if something else comes up. My weekly quota may not be reached by Friday evening; however, I'm happy to work on the weekend if it means I have freedom to surf on a Tuesday.
* _Creates separation between work and free time_. There's no more uncertainty or guilt surrounding when I should work. Once the daily limit is reached, I'm off. Once the weekly quota is reached, I check out.

This system has been awesome for my mental health and relationship with work. Funny enough, by setting a daily limit I'm actually more productive overall! [see the figure below]. See [this article](https://jamesclear.com/average-speed%29) discussing why "average speed" is more important than "peak speed."

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% responsive_image path: assets/img/2205_books/book_collage_v2.png title: "book collage" class: "img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Tomato productivity increased when I switched from system 1 to system 2.
</div>

<br>
# Conclusion
xxx

<br>
# Acknowledgments (sp?)
Thanks to [Lina Colucci](https://linacolucci.com) who initially inspired me to try the pomodoro method. Also shoutout to [Kyle Treige](https://kyletreige.com) who has been a kindred spirit as I've experimented to tune my system.
