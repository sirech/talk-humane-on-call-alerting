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

class: center middle full-width white 
background-image: url(images/logos.png)

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

vast majority of alerts triggers automatically

---

what is the source of noise in alerts?

---

accidental noise

---

example -> alert k8s pod restarts

---

warnings -> make them failures or ignore

---

the solution: delete them!

---

it's ok to delete stuff that doesn't bring value

---

Learning 1 -> Prune

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

---

history about itsm, a crappy bespoke tool that made everybody's life miserable

---

no auto closing

---

no grouping

---

counter example -> pagerduty

---

or opsgenie

---

Learning 4 -> use tools that support you

---

# This alert is not at the right level

---

alerts that don't tell a story about business impact

---

different concerns and levels of abstraction

---

the monitoring pyramid
analog to the testing pyramid

---

- synthetic
- RUM
- APM
- Infrastructure

---

dashboards can be top down from the high level down to the details

---

Learning 5 -> Consider the relative level of each alert

---

# Can't remember last time we adapted our alerts

---

finding the right threshold feels like a treasure hunt

---

split alerts!

---

threshold vs change% to have different angles

---

builds in top of:

reduce noise
automation
use a good tool

---

tuning has a dark side

---

I've been changing my mind about tuning

---

Constantly tuning monitors is treating the symptom, not the cause

---

Learning 6 -> While tuning is important, alerting only reflects the underlying state of a system

---

# The alerting good practices

---

# Alerting Good Practices

- Noisy alerts -> Relentlessly remove noise
- It's always been like this -> Don't get used to dysfunction
- We do things by hand around here -> Provision alerts with code
- Fighthing against the tools -> Use the right tooling
- This alert is not at the right level -> The alerting pyramid
- Can't remember last time we adapted our alerts -> Constant tuning

---

# What about Handling those Alerts?

---

a different topic for a different presentation

---

still, a more balanced approach to alerts has reduced the stress levels of the teams I've been on

---

# What is the message here?

---

automation, discipline and lack of conformism

---

links

