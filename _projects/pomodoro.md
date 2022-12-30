---
layout: page
title: pomodoro system design
description: establishing a more productive, balanced lifestyle
img: assets/img/2212_pomodoro/tom_header.png 
importance: 1
category: .
---

<br>

Years ago I was frustrated with how I worked, suffering from undirected time at my desk and undefined boundaries between work and personal time. 
I was not the efficient and balanced human I wanted to be.

Initially a quest to become more focused, this project completely shifted my orientation toward work&mdash;enabling me to establish boundaries and accept my finitude, despite always having more to do.
Hopefully this journey toward improved productivity and balance can offer a nugget of insight for your own.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-9 mt-3 mt-md-0">
        {% responsive_image path: assets/img/2212_pomodoro/tom_header.png title: "tomatoes" class: "img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
"Pomodoro" is Italian for tomato (credit: iStock).
</div>
<br>

# Pomodoro Method
The [pomodoro method](https://en.wikipedia.org/wiki/Pomodoro_Technique) is a time-management technique which separates work into fixed intervals, referred to as "tomatoes" (pomodoro is Italian for tomato). Typical length for one tomato is 25 minutes. Critically, this time must be committed to pure work; if one becomes distracted or interrupted, the rule is to "squash" that tomato and re-start the clock. Personally, turning off notifications has been indispensable for avoiding squashes.

To track tomatoes, I use [this simple web tool](https://mytomatoes.com/), which breaks one interval into three steps: (1) start the timer (2) work for 25 minutes (3) record what you did. It also provides the option for a five-minute break; personally I prefer to complete 4-8 consecutive tomatoes before taking a longer rest.

<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% responsive_image path: assets/img/2212_pomodoro/tom_demo.png title: "pomodoro_demo" class: "img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Sequential steps of one tomato interval using my favorite <a href="https://mytomatoes.com/">pomodoro tracker</a>.
</div>
<br>

There are many pomodoro trackers out there, but I really like this one prompting "what did you do?" It forces me to pop my head up and briefly consider how my current sub-task contributes toward the overall project. Many unproductive rabbit holes have been avoided this way. 
For fun, I've extracted all my responses from this past year to make a word cloud depicting how I use my time (see below).

Another unexpected benefit is that starting a tomato can help me overcome the activation energy for beginning work. Say I'm feeling tired, or there's a task I just don't feel like doing. I'll negotiate with myself: "spend one tomato on this task; if afterward you're still not in the mood, you can stop." 
Almost always I'll find my groove and feel motivated.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% responsive_image path: assets/img/2212_pomodoro/wordcloud.png title: "word_cloud" class: "img-fluid rounded z-depth-0" %}
    </div>
</div>
<div class="caption">
    Word cloud generated from responses to "what did you do?" across 3000+ tomatoes in 2022. 
<br> To help create your own, here's my <a href="https://github.com/davevanveen/data_viz/blob/main/tomato_wordcloud.py">python script</a>.
</div>
<br>

Now I'll discuss how I've integrated the pomodoro method on a daily and weekly basis, and how minor tweaks made a big difference for mental health and relationship with work.

<br>

# System 1: daily quota
Initially I employed a simple system: each day, accomplish $$\geq n$$ tomatoes. This number may be tuned depending on the week, but typically $$n=16$$. Essential meetings count as one tomato; this incentivizes shorter, efficient meetings and still makes the quota attainable on days my calendar is jammed. 

Overall this system empowered me to develop focus. However, it worked poorly in these two scenarios:
* _Not reaching $$n$$_. Some days life pulls in a different direction. Clinging to a minimum feels overly rigid when other opportunities arise.
* _Far exceeding $$n$$_. There's always more work, and I struggle knowing when to stop. Some days I'd do an unsustainable 25+ tomatoes. This often created an energy deficit which later I'd need to reconcile.

<br>
# System 2: weekly quota + daily cap
Instead of a lower bound for the day, I now set one for the week ($$\geq 5n$$ weekly) in addition to an upper bound each day ($$ \leq n$$ daily). That is, if I achieve $$n$$, I must stop working that day. This system has been a huge improvement, and here's why:
* _Incentivizes an early start_. Beginning my work day sooner creates more space later in the day.
* _Motivates quick bursts_. I'm eager to capitalize on short windows of time, e.g. given 30 minutes between meetings. 
* _Creates a clear end-of-day_. Once my daily cap is reached, I feel confident surrendering undone tasks.
* _Spreads responsibility across the week_. It's easier to accept lighter work days when something else comes up. My weekly quota usually isn't reached by Friday, but I'm happy to work on the weekend if it means I can surf guilt-free on a Tuesday.

Funny enough, by setting a daily limit, I'm actually more productive overall (see graph below). [Here's an article](https://jamesclear.com/average-speed%29) discussing why "average speed" is more important than "peak speed."

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% responsive_image path: assets/img/2212_pomodoro/tom_graph.png title: "pomodoro_graph" class: "img-fluid rounded z-depth-0" %}
    </div>
</div>
<div class="caption">
    Overall productivity is <i>higher</i> when I set a daily cap on tomatoes.
Shown here is my weekly tomato output from the last six weeks of system 1 (daily quota) and first six weeks of switching to system 2 (weekly quota + daily cap).
<br> Not shown: an improvement in well-being.
</div>
<br>

# Conclusion
The pomodoro method is a great productivity tool. More importantly for me, though, is that it provides a platform to actively improve my relationship with work.
As a result, I've found it easier to accept my place as a finite creature surrounded by endless possibility.

If you're curious to try, I recommend starting small, e.g. a single session of 1-2 tomatoes. Perhaps design experiments and record observations to learn what works for you.
With so much of our lives spent working, bringing intentionality to _how_ we work seems immensely important.

<br>
# Acknowledgments
Thanks to [Lina Colucci](https://linacolucci.com) who initially inspired me to try the pomodoro method. 
I'm also grateful to [Kyle Treige](https://kyletreige.com), a close friend and kindred spirit in designing systems of productivity.
