title: Humane On-Call: Alerting doesn't have to be painful
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
]

---

class: impact full-width

.impact-wrapper[
# Why?
]

---

people expect websites to be up at all times these days

---

mention to FB downtime on 04.10

---

99.95% ("three and a half nines")	4.38 hours	21.92 minutes

.link[https://en.wikipedia.org/wiki/High_availability]

---

hard to achieve if engineering is only available during business hours

---

class: impact full-width

.impact-wrapper[
# On-Call in a Nutshell
]

???

- And thus, on-call was born

---

monitor production systems


---

24x7

---

Rotations

???

- one developer at a time
-- maybe a secondary
- weekly rotations
- carrying a pager

---

You build it, you run it

.link[https://www.stevesmith.tech/blog/you-build-it-you-run-it/]

---

class: impact full-width

.impact-wrapper[
# On-Call can suck
]

???

- if you have been part of a rotation you probably know what I mean

---

many things can go wrong, I'm focusing on the alerting side

---

class: middle

## Constant Interruptions

![interruptions](images/interruptions.png)

---

class: middle

## Bad Night

.image-grid[
![call](images/call.png)
![sms](images/sms.png)
]

---

class: middle

## Unhelpful Alerts

![unhelpful-alarm](images/unhelpful-alarm.png)

---

picture -> explosion

---

we can do better than this

---

# Side Note

it's surprisingly difficult to find reliable numbers about this topic

---

# Alerting Dysfunctions

- Noisy alerts
- It's always been like this
- We do things by hand around here
- Fighthing against the tools
- This alert is not at the right level
- Can't remember last time we adapted our alerts

---

# Alerting Good Practices

- Noisy alerts -> Relentlessly remove noise
- It's always been like this -> Don't get used to dysfunction
- We do things by hand around here -> Provision alerts with code
- Fighthing against the tools -> Use the right tooling
- This alert is not at the right level -> The alerting pyramid
- Can't remember last time we adapted our alerts -> Constant tuning

---

class: transition

## Mario Fernandez
 
### Staff Engineer
### Wayfair

---

class: center impact

# What Is the Goal of Alerting?

---

Detect problems in production before your customers do

.link[https://hceris.com/monitoring-alerts-that-dont-suck/]

---

## Avoid false negatives

Trigger when there's a problem

---

## Avoid false positives

Don't trigger for non-issues

---

## The three golden signals

Rate - Number of requests
Error Rate - % of failed requests
Duration - Time to fulfill a request

---

class: center impact

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

# It's Always Been Like This

---

yeah that alert always triggers I ignore it

---

we know this doesn't work but we can't do anything about it

---

a personal favorite -> VMs degrade performance over time until they are unusable, keep track of convos in slack to restart it manually

---

PostMortem items ignored -> clear, unequivocal priority

---

alerting is not just a technical artifact, it requires organizational alignment

---

Learning 2 -> commit to act

---

class: center impact

# We do things by hand around here

---

automation is crucial in alerting

---

Benefits

- mainteinance
- spreading best practices
- granularity

---

any serious monitoring provider has APIs

---

example -> terraform alert

---

Encoding best practices through automated alerts

---

Learning 3 -> Automation is the only way to maintain alerts at scale

---

# Fighting against the tools

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

-> alerting good practices slide

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

