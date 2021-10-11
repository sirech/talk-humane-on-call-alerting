title: Humane On-Call
class: animation-fade
layout: true


<!-- This slide will serve as the base layout for all your slides -->

---

# Description (not part of the presentation)

Do you believe in “you build it, you run it”? What if you have on-call rotations, where you are responsible 24x7 for the health of a system? Nothing is quite so infuriating as a collection of poorly structured alerts that trigger randomly.

So, let’s do better! I want to talk about how to improve your monitoring capabilities. There are a few topics I want to touch:

- Reduce the noise
- Automate as much as possible
- Build actionable triggers
- Tune your monitoring constantly

After this talk, you’ll have concrete actions to make your engineers’ life easier when on-call

---

# Elevator Pitch (not part of the presentation)

A lot of companies want to be like Amazon today. DevOps, you build it you run it, and other practices are supposed to put you at the top of the technology pyramid.

On-Call is an increasing reality for developers, especially when  a site has strict uptime requirements. And sadly, the experience often sucks. It’s easy to mandate 24x7 support, it’s much harder to set it up in a way that doesn’t make the life of the people in the rotation miserable.

I want to talk about making that experience bearable. I’ve seen developers struggle with the operational burden over three companies already, and I think there are many actionable suggestions to improve it and build something sustainable.

---

class: impact full-width

.impact-wrapper[
# {{title}}
## Alerting doesn't have to be painful
]

---

class: center middle

# On-Call **is** a reality of operating production software

---

class: center middle

# Let's make it as bearable as possible!

???

The message of this talk is:

- We must make it as painless as possible
- We can improve things

---

class: impact full-width

.impact-wrapper[
# On-Call in a Nutshell
]

???

- What is actually on-call?

---

class: center middle

# ❌ Constantly monitor production

--

# ✅ Being available to handle an incident

???

- It's not an active thing.
- It's reactive. You're available in case you're needed
-- NOT for any incident though

---

class: center middle

# 24x7

---

class: center middle

# As part of a _rotation_

---

class: center middle

# Primary/Secondary developer on call 
    
# Weekly rotations 

# Carry a pager 

???

- how could a rotation look like? Let's think about it

---

class: impact full-width

.impact-wrapper[
# Why do this to ourselves?
]

---

class: center middle

# People expect constant availability

---

<!-- .slide: data-background-image="images/logos.png" data-background-size="100% auto" -->

???

- Maybe Facebook is not the best example right now, given the massive downtime they had

---

class: center middle

# High Availability (99,95%)
## 21.92 minutes / month
## 4.38 hours / year

.bottom-right[https://en.wikipedia.org/wiki/High_availability]

???

- 3 and half nines is what Google SQL offers
- 20 minutes per month isn't a lot! If you rollback manually and the pipeline isn't fast, it could take more than that

---

class: center middle

# 9 to 5 ain't highly available

---

class: center middle

# You build it, you run it

.link[https://www.stevesmith.tech/blog/you-build-it-you-run-it/]

???

- this model is born to provide faster support for individual services
- less handovers
- developers have "skin in the game"

---

class: impact full-width

.impact-wrapper[
# On-Call can suck
]

???

- if you have been part of a rotation you probably know what I mean

---

class: center middle

# This talk focuses on the alerting side
## *Many* other things can go wrong

---

class: center full-width 
background-image: url(images/interruptions.png)

# Constant Interruptions

???

- what if your alerts are ringing constantly
- can be extremely disruptive to your day-to-day work
- even moreso if the person on-call keeps working on the regular topics

---

class: center middle

# Bad Night(s)

.image-grid[
![call](images/call.png)
![sms](images/sms.png)
]

???

- alerts during working hours are actually the *good* case
- when the alerts start calling you at night is when life gets really miserable

---

class: center right full-height
background-image: url(images/flaky-alerts.png)

# Flakyness

???

- this alert triggered and resolved within two minutes
- if you get waken up, it takes longer than that to get to a computer

---

class: center full-width
background-image: url(images/unhelpful-alarm.png)

# Not Actionable

???

- Alerts that don't have a clear action are specially annoying
- TODO: not sure if this slide fits here

---

class: center full-width
background-image: url(images/explosion.jpeg)

---

class: center middle

# We can do better than this

---

class: center middle

# Side Note
## Are there reliable numbers about on-call out there?

---

class: impact full-width

.impact-wrapper[
# Alerting Dysfunctions
]

???

- as I said I want to talk about alerting
-- the on-call topic is much more than alerts
- I'll be focusing the rest of the talk on alerting dysfunctions
-- What are they
-- Hwo to fix it

---

class: middle

## Noisy alerts
## It's always been like this
## We do things by hand around here
## Fighting against the tools
## This alert is not at the right level
## Can't remember last time we adapted our alerts

---

class: transition

## Mario Fernandez
 
### Staff Engineer
### Wayfair

---

class: center impact

# Alerting 101

---

class: center middle

# Detect problems in production before your customers do

.bottom-right[https://hceris.com/monitoring-alerts-that-dont-suck/]

---

class: center middle

# Avoid false negatives
## _Trigger when there's a problem_

---

class: center middle

# Avoid false positives
## _Don't trigger alarms for non-issues_

---

class: center middle

# The Four Golden Signals

---

class: middle

## Latency
## Traffic
## Errors
## Saturation

.bottom-right[https://sre.google/sre-book/monitoring-distributed-systems/]

---

class: center transition

# Noisy Alerts

---

class: center middle

# Vast majority of alerts trigger automatically

???

- By that I mean that #automated alerts >>> #manually reported alerts
- If you're not in that situation, not everything I'm saying will apply to you

---

class: center middle

# A lot of the noise is *accidental*

???

- Akin to the concept of accidental complexity
-- A certain amount of complexity is unavoidable
-- However, the wrong architecture can introduce avoidable complexity

---

class: center middle

# Triggering an alert when a pod in Kubernetes restarts

???

- Kubernetes restarts pods all the time without intervention. It's kind of the point of using it
- Maybe a more accurate statement is that trying to monitor how k8s works is a recipe for frustration

---

class: center middle

# Warnings are evil
## Make them failures or ignore them

???

- That's a strong statement, so I'm sure there are exceptions
- Still, making decisions more binary helps reducing the noise

---

class: center middle

# Delete things you don't need!

---

class: center middle

# I'm serious, delete them and thank me later

---

class: center middle

# Learning 1️⃣  ➡️ Be ruthless in reducing the noise

---

class: center transition

# It's Always Been Like This

---

class: center middle

# When that alert wakes me up I just snooze it and keep sleeping

???

- This is an actual quote, I'm not making it up
-- Happened in a previous project that I joined as a Tech Lead
- I've seen it where I work now. People get used to things that *are not* okay

---

class: center middle

# We know this doesn't work, but we can't do anything about it

???

- You know how I mentioned "you build it you run it" before?
- That only works if you actual agency to change
- Are you a proxy to another team? That's a bad position to be in

---

class: right full-width
background-image: url(images/slowing-monolith.png)

# The slowing monolith

???

- this a personal favorite
-- a service of us depends on an old monolith. For unknown reasons, it degrades over time until the performance is unusable
-- infra folks need to restart the VMs every now and then
- for organizational reasons, we don't have the rights to do that, and we can't even automate the process of restarting to mitigate the pain at least

---

class: center middle

# Ignoring Post Mortem items 😢

???

- I'm assuming that you do post mortems after incidents
-- If not, well that's the action point
- Even if you do them, you have to commit to prioritize them

---

class: center middle

# Alerting isn't just about technology. It's about organizational alignment

???

- To quote a former colleague: "You don't have a technology problem, you have an organizational one"
- You notice that this part has nothing to do with tools or infrastructure

---

class: center middle

# Learning 2️⃣  ➡️ Commit to action

---

class: center transition

# We Do Things by Hand Around Here

---

class: center middle

# Automation is crucial in alerting

---

class: middle

## Maintenance

--

## Granularity

--

## Spread best practices

---

class: center middle

## Any serious monitoring provider allows automation

.bottom-right[https://registry.terraform.io/browse/providers?category=logging-monitoring]

---

class: center middle

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

class: center middle

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

---

class: center middle

# From 1 to N to N^(a lot)

???

- If you want to monitor things deeply, you'll end up with a lot of alerts
- Carrying changes is impossible if you have to do it by hand

---

class: center middle

# Learning 3️⃣  ➡️ Automation is the only way to maintain alerts at scale

---

class: center transition

# Fighting Against the Tools

???

- I've said in point 2 that technology alone is not the solution
- However, good tooling has a clear place in alerting: Make your life easier
- Two features that I want to mention

---

class: center middle

# Auto closing

???

- if the alert gets opened and closed, it should be reflected in the tool and not add to the noise

---

class: center middle

# Auto grouping

???

- If the same alert or similar group of alerts get triggered, that should be clearly grouped and not treated as individual alerts

---

class: center middle

# Conditional routing
## Based on time (business hours vs off hours)
## Priority (critical vs minor)

???

- Routing based on conditions
-- some alerts can wait until the next day
-- some alerts require immediate action. Others can be logged as tickets

---

class: center middle

# Do you take it for granted? I've learned not to

???

- Counter example: A tool called ITSM, in a project for a client that I won't mention
- It had none of these features
- I wish I had a screenshot of the tool because you would understand instantly what I mean

---

class: center middle

.image-grid[
![datadog](images/datadog.png)
![pagerduty](images/pagerduty.png)
]

???

- Bottom line, use good tools. They're worth it
- I mention DD and PD here because I'm familiar with them. There are many alternatives

---

class: center middle

# Learning 4️⃣  ➡️ Use tools that support you

---

class: center transition

# This Alert Is not at the Right Level

---

class: center middle

# What's the business impact?

---

class: center middle

# Different concerns and levels of abstraction

---

class: center middle full-height
background-image: url(images/monitoring-pyramid.png)

???

- What if we consider the different sources of monitoring like we do the testing pyramid?
- Simulating user traffic goes at the top -> synthetics
- We go do down progressively, like RUM or APM
- At the bottom, the monitoring of each individual piece

---

class: center middle full-height
background-image: url(images/dashboard.png)

???

- One think that I've been doing lately is building dashboards top to bottom. By that I mean start with general metrics, user perspective and go down and drill into individual services
- If you're answering an alert, the first question has to be: Do I need to do something?

---

class: center middle

# Learning 5️⃣  ➡️ Consider the abstraction level

---

class: center transition

# Can't Remember the Last Time we Tuned our Alerts

---

class: center middle

# Finding the right threshold feels like a treasure hunt

---

class: center middle

# Don't try to find the perfect alert 
## Smaller alerts that cover different angles

---

class: center middle

# Example: Threshold vs Change%

???

- A very concrete example: thresholds vs change%

---

class: center middle

# Tuning builds on top of
## Reducing noise
## Automation
## Using the right tool

---

class: center middle

# Tuning alerts is treating the symptom

???

- I've been changing my mind about tuning
- There's only so much you can do
-- If the system is unstable, good alerting will reflect that

---

class: center middle

# Learning 6️⃣  ➡️ Alerting only reflects the underlying state of a system

---

class: center impact

# Let's summarize the learnings

---

class: middle

## Noisy alerts
## It's always been like this
## We do things by hand around here
## Fighting against the tools
## This alert is not at the right level
## Can't remember last time we adapted our alerts

---

class: middle

## Relentlessly remove noise
## Don't get used to dysfunction
## Provision alerts with code
## Use the right tooling
## The alerting pyramid
## Constant tuning

---

class: center middle

# Sensible alerts are a good starting spot

---

class: center middle

# But not an end!

???

- Even the most accurately tuned alerts will trigger
-- How to handle them?
-- How to improve?
- The topic is big enough to cover book(s)

---

class: center middle full-width
background-image: url(images/sre.png)

---
