---
title: Who needs users in this brave cloud-centric world?
pdf: who-needs-users.pdf
slideNumber: true
controls: false
transition: slide
backgroundTransition: fade
asciinema: true
center: true
---

<link href="//netdna.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">

# <small>WHO NEEDS USERS<br/>IN THIS BRAVE **CLOUD NATIVE** WORLD?</small> {bgcss=tw-colorful .light-on-dark}

```render_a2sketch
.---------------------------------------------.
|[c]                                          |
|                                             |
|                                             |
|                                             |
|                                             |
'---------------------------------------------'

[c]: {"a2s:type": "cloud", "a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
This is an attempt to examine a worrying trend to every greater complexity in the Kubernetes world.
</aside>

# END USERS BEWARE

```render_a2sketch
      #-----------------------------------------------------------------------#
      |[q]                                                                    |
      |                                                                       |
      |                                                                       |
      |                                                                       |
      |                                                                       |
      |    Kubernetes is for people building platforms. If you are a          |
      |                                                                       |
      |    developer building your own platform (AppEngine, Cloud Foundry,    |
      |                                                                       |
      |    Heroku clone), then Kubernetes is for you.                         |
      |                                                                       |
      |                                                 - Kelsey Hightower    |
      |                                                                       |
      |                                                                       |
      |                                                                       |
      |                                                                       |
      #-----------------------------------------------------------------------#

[q]: {"a2s:type": "quote-sw", "a2s:delref": true, "fill": "#ef5ba1", "fillStyle": "solid"}
```

<div class="tiny">Source: <a href="https://twitter.com/kelseyhightower/status/1099710178545434625">@kelseyhightower on 24 Feb 2019</a>.</div>

<aside class="notes" data-markdown>
It's worth stressing that nobody has done more to bring users to the Kubernetes platform than Kelsey Hightower. He also doesn't mention end users here, though it's probably fair to say he implies Kubernetes is not for them.
</aside>

# ALL HAPPY FAMILIES {bgcss=tw-bg-light-blue .light-on-dark}

```render_a2sketch
.--------------------------------------------------------------.    
|[c]                                                           |    
|                                                              |    
|           Docker Swarm                                       |
|                                 OpenShift (1-2)              |    
|                      Kubernetes                              | 
|     Apache Mesos                                             | 
|                             Elastic Container Service        | 
|            Heroku                                            | 
|                    Cloud Foundry                             | 
|                                   Panamax                    | 
|          Shipyard                                            | 
|                                       Portainer              | 
|                                                              |    
'--------------------------------------------------------------'    

[c]: {"a2s:type": "cloud", "a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
This position (there are platforms for end users and others for platform builders) is absolutely fine in a vibrant ecosystem of container schedulers. There's no doubt developers love Docker Swarm, for example.
</aside>

# SPLENDID ISOLATION {bgcss=tw-bg-light-blue .light-on-dark}

```render_a2sketch
.--------------------------------------------------------------.
|[c]                                                           |
|                                                              |
|                                                              |
|                                                              |
|                      Kubernetes                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
'--------------------------------------------------------------'

[c]: {"a2s:type": "cloud", "a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
However, that is not the ecosystem we have today. It seems odd to let Kubernetes displace all contenders only to say that it's for platform builders only.
</aside>

# PIZZA EFFECTS: IDENTITY

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : attribute-based access control (ABAC)
OpenShift->Kubernetes : roles and cluster-roles
Kubernetes->OpenShift : role-based access control (RBAC)
@enduml
```

<aside class="notes" data-markdown>
Since Red Hat embraced Kubernetes for OpenShift 3+, there have been numerous virtuous pizza effects, large and small. RBAC is an example that has worked particularly well.
</aside>

# PIZZA EFFECTS: NETWORKING

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : flat network
OpenShift->Kubernetes : multitenant plugin
OpenShift->Kubernetes : prototype network policies
Kubernetes->OpenShift : network policies
@enduml
```

<aside class="notes" data-markdown>
Network policies have arguably been less successful. The question is whether network policies warrant the considerable amount of complexity when placed alongside Red Hat's original `ovs-multitenant` plugin.
</aside>

# PIZZA EFFECTS: GENERIC 

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : feature unavailable
OpenShift->Kubernetes : imperative API calls
Kubernetes->OpenShift : declarative API objects 
@enduml
```

<aside class="notes" data-markdown>
In very general terms, OpenShift fills an important functionality gap. Typically an imperative API is supplied via `oc`; the version adopted upstream tends to be more generic, more powerful and far more complex.
</aside>

# MULTITENANCY {bg=#6a2469 .light-on-dark}

```render_a2sketch
#=----------------------------#
|[w]                          |
| cluster                     |
|                             |
| #----------#   #----------# |
| |[p]       |   |[b]       | |
| | boeing   |   | airbus   | |
| |          |   |          | |
| |          |   |          | |
| #----------#   #----------# |
|                             |
|                             |
|                             |
|                             |
|                             |
|                             |
|                             |
|                             |
#-----------------------------#

[p]: {"a2s:delref": true, "fill": "#ef5ba1", "fillStyle": "solid"}
[b]: {"a2s:delref": true, "fill": "#27bdce", "fillStyle": "solid"}
[w]: {"a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
Here's the basic principle of the `ovs-multitenant` plugin: namespace boundaries are an excellent point of separation for workloads owned by different organisations or organisational groupings. The default is full isolation with the proviso that the `default` namespace is marked `global`.
</aside>

# ENABLE ROUTER ACCESS {bg=#6a2469 .light-on-dark}

```render_a2sketch
#=----------------------------#
|[w]                          |
| cluster                     |
| #----------#   #----------# |
| |[p]       |   |[b]       | |
| | boeing   |   | airbus   | |
| |          |   |          | |
| |          |   |          | |
| #----+-----#   #-----+----# |
|      ^               ^      |
|      |               |      |
|      v               v      |
| #----+-----#   #-----+----# |
| |          |   |          | |
| | router-a |   | router-b | |
| |          |   |          | |
| |          |   |          | |
| #----------#   #----------# |
#-----------------------------#

[p]: {"a2s:delref": true, "fill": "#ef5ba1", "fillStyle": "solid"}
[b]: {"a2s:delref": true, "fill": "#27bdce", "fillStyle": "solid"}
[w]: {"a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

```shell
$ oc adm pod-network join-projects --to router-a boeing
$ oc adm pod-network join-projects --to router-b airbus
$ oc get netnamespace airbus boeing router-a router-b
NAME          NETID
airbus        1064346
boeing        1086658
router-a      1064346
router-b      1086658
```

<aside class="notes" data-markdown>
Here is how `ovs-multitenant` users set up connections between namespaces. The output of `oc get netnamespace` also conveniently shows the inner workings of the plugin: two namespaces with the same NETID can communicate with one another. (NETID 0 is special as it marks global namespaces.)
</aside>

# SELECTIVE ISOLATION {bg=#6a2469 .light-on-dark}

```render_a2sketch
#=----------------------------#
|[w]                          |
| cluster                     |
| #----------#   #----------# |
| |[p]       |   |[b]       | |
| | boeing   |   | airbus   | |
| |          |   |          | |
| |          |   |          | |
| #----+-----#   #----------# |
|      ^                      |
|      |                      |
|      v                      |
| #----+-----#   #----------# |
| |          |   |          | |
| | router-a |   | router-b | |
| |          |   |          | |
| |          |   |          | |
| #----------#   #----------# |
#-----------------------------#

[p]: {"a2s:delref": true, "fill": "#ef5ba1", "fillStyle": "solid"}
[b]: {"a2s:delref": true, "fill": "#27bdce", "fillStyle": "solid"}
[w]: {"a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

```shell
$ oc adm pod-network isolate-projects airbus
$ oc get netnamespace airbus boeing router-a router-b
NAME          NETID
airbus        1020181
boeing        1086658
router-a      1086658
router-b      1064346
```

<aside class="notes" data-markdown>
This is how connections can be severed. There are serious problems with the plugin's imperative API - the outcome of API calls is often unintuitive, but that is for another discussion - but the approach has the virtue of simplicity.
</aside>

# NETWORK POLICIES {bg=#fff44d}

<img src="assets/images/goal.svg" alt="schematic of a typical pod network"/>

<aside class="notes" data-markdown>
However, the control afforded by the multitenant plugin was not deemed sufficient. Instead, the upstream project adopted a solution quite close to the declarative policy approach Red Hat proposed next.

The key difference is that the isolation boundary is no longer the namespace but the pod.
</aside>

# AUDIT {bg=#fff44d}

<img src="assets/images/network-policy-viewer.png" alt="screenshot of network policy viewer"/>

<div class="tiny">Source: <a href="https://github.com/gerald1248/k8s-network-policy-viewer">github.com/gerald1248/k8s-network-policy-viewer</a></div>

<aside class="notes" data-markdown>
Why should I care? I stumbled upon this as I was trying to obtain some metrics (percentage of namespaces covered by network policies, that kind of thing) from a cluster's network policies. My expectation was that this would be a quick task, but that expectation proved to be overly optimistic.
</aside>

# IMPLEMENTATION {bgcss=tw-bg-light-blue .light-on-dark}

```render_a2sketch
.--------------------------------------------------------------.
|[c]                                                           |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                 Somebody else's problem                      |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
|                                                              |
'--------------------------------------------------------------'

[c]: {"a2s:type": "cloud", "a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
One problem, arguably, is that the spec was written by one party, to be implemented by another. The authors of the spec had little incentive to plump for something simple and lacking in features.
</aside>

# WHAT THE USER SAW {bg=#000 .light-on-dark}

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-whitelist-pod
spec:
  podSelector:
    matchLabels:
      app: httpd-bob
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: httpd-alice
```

<aside class="notes" data-markdown>
This is a typical network policy. Coming from the multitenant plugin, I felt this was everything I had wished for. Out with the messy imperative API, in with the familiar look of a Kubernetes manifest.
</aside>

# USER ALL AT SEA (1) {bg=#000 .light-on-dark}

```yaml
podSelector: {}
```

<small>is the opposite of:</small>

```yaml
podSelector: []
```

<aside class="notes" data-markdown>
This is a very minor issue (and really a point of YAML syntax rather than NetworkPolicy objects), but it still leads to some policies in the wild whose effect is the exact opposite of what the author intended. What's more, both selectors are entirely idiomatic.
</aside>

# USER ALL AT SEA (2) {bg=#000 .light-on-dark}

```yaml
ingress:
- from:
  - namespaceSelector:
      matchLabels:
        user: alice
    podSelector:
      matchLabels:
        role: client
```

<small>is not at all the same as:</small>

```yaml
ingress:
- from:
  - namespaceSelector:
      matchLabels:
        user: alice
  - podSelector:
      matchLabels:
        role: client
```

<div class="tiny">Source: <a href="https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors">kubernetes.io/docs</a>.</div>

<aside class="notes" data-markdown>
This again is a YAML issue first and foremost, but it is worth asking why both `ingress` *and* `from` are arrays; and this before the user has run into the issue illustrated here.

The problem is not so much the potential for confusion (substantial though that is) but the difficulty faced by anyone trying to make sense of dozens of network policy objects in the event of an outage. The scope for misreading is immense and the complexity of disentangling many network policies spread out across multiple namespaces doesn't bear thinking about.
</aside>

# WHAT THE PARSER DID

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
scale 550 height
(*) -down-> "NetworkPolicy object found"
note right: let's ignore policies in other namespaces for now
-down-> "Which pods in the namespace are affected by this policy?"
--> "policy type Ingress set (or none)?"
if "" then
  -->[true] "block all incoming traffic"
  --> "whitelist any pods/namespaces selected under ingress.from"
  --> "policy type Egress set?"
else
  ->[false] "policy type Egress set?"
endif

if "" then
  -->[true] "block all outgoing traffic"
  --> "whitelist any pods/namespaces selected under egress.to"
  --> "other NetworkPolicy objects in the namespace?"
else
  -->[false] "other NetworkPolicy objects in the namespace?"
endif
  
if "" then
  -->[true] All bets are off
  -->"NetworkPolicy object found"
else
  -->[false] (*)
endif

@enduml
```

<aside class="notes" data-markdown>
It is worth stepping through this flow from start to finish; note that the main source of complexity (policies whose effects affect pods in other namespaces) is explicitly omitted from the chart. 
</aside>

# USER PREFERENCE

```render_a2sketch
         ^
features |                                    • NetworkPolicy
         |
         |
         |                    
         |                   
         |  .-----------------.
         |  |[c]              |
         |  |• ovs-multitenant|
         |  |                 |
         |  '-----------------'
         |                                        good enough
         |    --    --    --    --    --    --    --    --   
         |                                        won't work
         |                                                   
         |                                                   
         |                                                   
         |                                                   
         |
         |
         #--------------------------------------------------->
                                                   complexity

[c]: {"a2s:delref":true,"a2s:type":"circle"}
```

<aside class="notes" data-markdown>
It seems likely that for many cluster administrators, the less capable but much simpler `ovs-multitenant` option pioneered by OpenShift 3+ is the preferred option.
</aside>

# ENTER OPERATOR {bg=#000 .light-on-dark}

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: boeing
  annotations:
    networking.k8s.io/netnamespace-id: "100"
---
apiVersion: v1
kind: Namespace
metadata:
  name: router-a
  annotations:
    networking.k8s.io/netnamespace-id: "100"
```

<aside class="notes" data-markdown>
Here is one possible solution that marries declarative YAML with the approach of the `ovs-multitenant` plugin. There is no need for an imperative API if we annotate the namespace objects and let an operator manage matching NetworkPolicy objects. Anyone who is happy to stick to namespace boundaries for isolation should be well served by this approach.
</aside>

# BEWARE ROUGH EDGES

```render_a2sketch
#------------------------------------------------------------------#
|[q]                                                               |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|    I'm a little bit sad how complex Kubernetes is right now.     |
|                                                                  |
|    In some sense Kubernetes is not for end users, it's for       |
|                                                                  |
|    people who set up clusters and clusters are for end users.    |
|                                                                  |
|                                               - Wojtek Cichoń    |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
#------------------------------------------------------------------#

[q]: {"a2s:type": "quote-se", "a2s:delref": true, "fill": "#27bdce", "fillStyle": "solid"}
```

<div class="tiny">Source: Cichoń, W. 2018. <a href="https://thenewstack.io/kubernetes-pioneer-tim-hockin-on-whats-real-and-whats-not">What's real and what's not</a>.</div>

<aside class="notes" data-markdown>
This strikes me as a constructive and helpful take on the point made by Kelsey Hightower on the opening slide. It's fair to make a distinction between cluster administrators and cluster users, but that isn't to say that all cluster administrators are platform builders.
</aside>

# <small>THANK YOU</small> {bgcss=tw-colorful .light-on-dark}

<small>Slides built with <a href="https://github.com/arnehilmann/markdeck">Markdeck</a><br/>
<i class="fa fa-github" aria-hidden="true"></i> <a href="https://github.com/gerald1248/who-needs-users-slides">gerald1248/who-needs-users-slides</a><br/>
<i class="fa fa-twitter" aria-hidden="true"></i> 03spirit</small>


<img src="assets/images/ThoughtWorks_logo_white.png" alt="ThoughtWorks logo" width="25%"/>
