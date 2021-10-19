<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Humane On-Call
## Alerting doesn't have to be painful

???

- before I get started, how many of you are part of a regular on-call rotation?

---

<h2 class="fragment fade-up">
On-Call is a reality of operating production software
</h2>

<h2 class="fragment fade-up">
Alerting is a classical friction point
</h2>

<h2 class="fragment fade-up">
Let's make it as bearable as possible!
</h2>


???

The message of this talk is:

- We must make it as painless as possible
- We can improve things

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# On-Call in a Nutshell

???

- What is actually on-call? Let's spend a bit of time to talk about it first

---

## ✅ Being *available* to handle an incident

???

- it can happen during business hours or outside of them

---

## ❌ Constantly monitor production

???

- It's not an active thing.
- It's reactive. You're available in case you're needed
- NOT for any incident though

---

## As part of a _rotation_

<img src="images/rotation.png" />

???

- Nobody should do on-call alone, although this happens sometimes, sadly
- weekly rotations

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Why do this to ourselves?

???

- An immediate question is: Why should we do this to ourselves?

---

## Users expect constant availability

---

<!-- .slide: data-background-image="images/logos.png" data-background-size="100% auto" -->

???

- Maybe Facebook is not the best example right now, given the massive downtime they had

---

## High Availability (99,95%)
### 21.92 minutes / month
### 4.38 hours / year

<span class=bottom-right>https://en.wikipedia.org/wiki/High_availability</span>

???

- 3 and half nines is what Google SQL offers
- 20 minutes per month isn't a lot! A slow rollback process for a bad deploy is enough to exhaust the budget

---

## You build it, you run it

<span class="bottom-right">https://www.stevesmith.tech/blog/you-build-it-you-run-it/<span>

???

- A second answer for "why do this to ourselves?"
- developers have "skin in the game"
- Build more resilient systems
- Reduce handovers

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Alerting 101

???

- on-call is a big topic
- I'm going to focus on alerting
- For that, let's do a short introduction to the topic

---

## Detect problems in production before your customers do

<span class="bottom-right">https://hceris.com/monitoring-alerts-that-dont-suck/</span>

---

## Monitoring ➕ Notification

---

## Avoid false positives
### _Don't trigger alarms for non-issues_

???

- Type I error

---

## Avoid false negatives
### _Trigger when there's a problem_

???

- Type II error

---

## The Four Golden Signals

### Latency
### Traffic
### Errors
### Saturation

<span class="bottom-right">https://sre.google/sre-book/monitoring-distributed-systems/</span>

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# ⚠️ 📟 🔥

???

- if you have been part of a rotation you probably know what I mean

---

<!-- .slide: data-background-image="images/interruptions.png" data-background-size="100% auto" -->

## Constant Interruptions

???

- what if your alerts are ringing constantly
- can be extremely disruptive to your day-to-day work
- even more so if the person on-call keeps working on the regular topics

---

## Bad Night(s)

![call](images/call.png)
![sms](images/sms.png)

???

- alerts during working hours are actually the *good* case
- when the alerts start calling you at night is when life gets really miserable

---

<!-- .slide: data-background-image="images/flaky-alerts.png" data-background-size="auto 100%" -->

<h2 class="right">Flakyness</h2>

???

- this alert triggered and resolved within two minutes
- if you get waken up, it takes longer than that to get to a computer

---

<!-- .slide: data-background-image="images/explosion.jpeg" data-background-size="100% auto" -- >

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Alerting Dysfunctions

???

- as I said I want to talk about alerting
-- the on-call topic is much more than alerts
- I've spent the past two years in full-time on-call, and have operated applications as a developer before that
- This are some of the dysfunctions I've found

---

### Noisy alerts
### Accepting the status quo
### Lack of automation
### Inadequate tools
### Mixed abstraction levels
### Mismatched tuning

---

<!-- .slide: data-background-color="var(--r-link-color-dark)"  -->

## Mario Fernandez
 
### Staff Engineer
### Wayfair


---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Noisy Alerts

---

## When the signal is drowned by the noise

---

todo -> pic either dashboard with too much info or cacophony of alerts

---

## Alert fatigue leads to ignoring alerts

<span class="bottom-right">https://www.atlassian.com/incident-management/on-call/alert-fatigue</span>

???

- this is completely expected
- you can't constantly be in tension

---

## A lot of the noise is *accidental*

???

- Akin to the concept of accidental complexity
-- A certain amount of complexity is unavoidable
-- However, the wrong architecture can introduce avoidable complexity

---

todo -> pic of kubernetes alert that is just a normal thing

???

- alerts where action isn't clear.
- Kubernetes restarts pods all the time without intervention. It's kind of the point of using it
- If it's expected, why monitor it?

---

todo -> alert with warning of some disk limit being reached

???

- alerts where urgency isn't clear.

---

## Warnings are evil
### Make them failures or ignore them

???

- That's a strong statement, so I'm sure there are exceptions
- Still, making decisions more binary helps reducing the noise

---

## Delete things you don't need!
### Like, seriously

---

## Learning 1️⃣  ➡️ 
### Reduce noise ruthlessly

???

- I feel like I'm pointing the obvious
- And yet, this is a pattern that keeps repeating wherever I go

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Accepting the Status Quo

---

## "It's always been like this"

???

- here are some actual quotes

---

> When that alert wakes me up I just snooze it and keep sleeping

???

- This is an actual quote, I'm not making it up
-- Happened in a previous project that I joined as a Tech Lead
- I've seen it where I work now. People get used to things that *are not* okay

---

> We know this doesn't work, but we can't do anything about it

???

- The opposite of you build it you run it
- You get the incidents but can't make the required changes

---

<!-- .slide: data-background-image="images/slowing-monolith.png" data-background-size="100% auto" -->

<h2 class="bottom-right">The slowing monolith</h2>

???

- this a personal favorite
- a service we own depends on a monolith that's not actively developed
- performance degrades over time until it reaches a breaking point (spikes in the chart)
- "the solution" is to restart the VMs, but it just pushes the problem to the future

---

## Don't fix organizational problems with technology

???

- Inspired by what a former colleague used to say all the time
- It's got nothing to do with tools

---

## Learning 2️⃣  ➡️ 
### Commit to action

???

- This starts at the post-mortem level
- you **have** to commit to carry the actions decided there, and do it swiftly

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Lack of Automation

???

- people get used to do things by hand

---

## Manual work means non-reproducible results

---

## Automation is crucial in alerting

???

- Why?

---

<h2 class="fragment fade-up">
Reduce maintenance
</h2>

<h2 class="fragment fade-up">
Higher granularity
</h2>

<h2 class="fragment fade-up">
Spread best practices
</h2>

---

![IaC](images/IaC.jpeg)

???

- monitoring/alerting is infrastructure

---

## Any serious monitoring provider allows automation

---

![terraform](images/terraform.svg)

<span class="bottom-right">https://registry.terraform.io/browse/providers?category=logging-monitoring</span>

---

```hcl
resource "datadog_monitor" "monitor" {
  count = try(var.enabled, false) == true ? 1 : 0

  name               = var.name
  type               = var.type
  query              = var.query
  message            = var.message

  monitor_thresholds {
    warning = lookup(var.monitor_thresholds, "warning", null)
    ok = lookup(var.monitor_thresholds, "ok", null)
    critical = lookup(var.monitor_thresholds, "critical", null)
  }

  priority            = var.priority
}
```

???

- This is simplified for space reasons, but you should get the idea

---

todo -> example PD?

---

```hcl
module "no_requests_flatline" {
  source = "../../monitor"

  enabled = var.enabled
  name = "${var.service_name} ${var.env} - Latency [Flatline]"
  query  = <<-EOT
avg(10m):p95:trace.servlet.request by {resource_name}
 > ${local.threshold}
EOT

  message = templatefile("${path.module}/message.md", {
    service = var.service_name
  })

  monitor_thresholds = { critical = local.threshold }
}
```

???

- based on this building blocks, you can create reusable modules

---

## Scale alerts inside a team

???

- If you want to monitor things deeply, you'll end up with a lot of alerts
- Carrying changes is impossible if you have to do it by hand

---

## Scale alerts across teams

???

- this is a problem in our organization right now
- a team learns from experience, but the learnings are rediscovered by other teams again

---

## Learning 3️⃣  ➡️ 
### Automation is the only way to maintain alerts at scale

???

- In my experience, nobody has disagreed openly about this 

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Inadequate Tools

???

- I've said in point 2 that technology alone is not the solution
- Time to contradict myself!
- Good tooling has a clear place in alerting: Make your life easier

---

> Crappy tools are extra infuriating at 3 in the morning

???

- I'm going to quote myself for this one

---

## Intersection of
### Monitoring
### Alerting

todo -> replace with picture?

???

- Two features that I want to mention

---

## Auto closing

???

- if the alert gets opened and closed, it should be reflected in the tool and not add to the noise

---

## Auto grouping

???

- If the same alert or similar group of alerts get triggered, that should be clearly grouped and not treated as individual alerts

---

## Conditional routing
### Based on time
### Priority

???

- Routing based on conditions
-- some alerts can wait until the next day
-- some alerts require immediate action. Others can be logged as tickets

---

## Do you take it for granted?
### I've learned not to 😩

???

- Counter example: A german client used an internal tool for incident management. It came straight out of the 90s
- It had none of these features
- I wish I had a screenshot of the tool because you would understand instantly what I mean

---

## The market is full of high-quality tools

---

![datadog](images/datadog.png)

---

![pagerduty](images/pagerduty.png)

???

- Bottom line, use good tools. They're worth it
- I mention DD and PD here because I'm familiar with them. There are many alternatives

---

## Learning 4️⃣  ➡️ 
### Use tools that support you

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Mixed Abstraction Levels

---

## What are you measuring?

???

- typical dashboard that measures very high level things (i.e: revenue), and very low level things (i.e: disk space)

---

<!-- .slide: data-background-image="images/synthetic.png" data-background-size="100% auto" -->

???

- If you have a synthetic flow, that answers questions about high level metrics

---

<!-- .slide: data-background-image="images/cpu-alert.png" data-background-size="auto 100%" -->

???

- An alert like cpu is high doesn't say anything about business impact
- It goes in both directions

---

<!-- .slide: data-background-image="images/monitoring-pyramid.png" data-background-size="auto 100%" -->

???

- What if we consider the different sources of monitoring like we do the testing pyramid?
- Simulating user traffic goes at the top -> synthetics
- We go do down progressively, like RUM or APM
- At the bottom, the monitoring of each individual piece

---

## Reflect the differences in abstraction level

???

- One thing that I've been doing lately is building dashboards top to bottom. By that I mean start with general metrics, user perspective and go down and drill into individual services

---

<!-- .slide: data-background-image="images/dashboard.png" data-background-size="auto 100%" -->

???

- If you're answering an alert, the first question has to be: Do I need to do something?

---

## Learning 5️⃣  ➡️ 
### Consider the abstraction level

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Mismatched Tuning

---

## Tuning alerts is tricky

---

## Tuning builds on top of
### Reducing noise
### Automation
### Using the right tool

---

## Balance between overly sensitive and missing incidents

---

## Avoid absolute thresholds

todo -> alert with arbitrary threshold

---

## Beware of low traffic services

todo -> dashboard for low traffic service

???

- solutions: aggregate services, create synthetic traffic

---

## Split alerts in smaller pieces

todo -> show flatline vs change%

???

- flatline
- change%

---

## However!
### Tuning alerts is treating the symptom

???

- I've been changing my mind about tuning
- There's only so much you can do
-- If the system is unstable, good alerting will reflect that

---

## Alerting only reflects the underlying state of a system

???

- I've found close to impossible to build reliable alerting systems on top of completely unstable systems

---

## Learning 6️⃣  ➡️ 
### Tune alerts constantly

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Let's summarize our learnings

---

### Noisy alerts
### Accepting the status quo
### Lack of automation
### Inadequate tools
### Mixed abstraction levels
### Mismatched tuning

---

### Reduce noise ruthlessly
### Commit to action
### Automation is the only way to maintain alerts at scale
### Use tools that support you
### Consider the abstraction level
### Tune alerts constantly

???

- is there an order? I'd say depends on your situation

---

## Sensible alerts are a good starting spot

---

## But not an end!

???

- Even the most accurately tuned alerts will trigger
-- How to handle them?
-- How to improve?
- The topic is big enough to cover book(s)

---

<!-- .slide: data-background-image="images/sre.png" data-background-size="100% auto" -->

---
