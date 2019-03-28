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
|    What's pizza got to do with anything?    |
|                                             |
|                                             |
'---------------------------------------------'

[c]: {"a2s:type": "cloud", "a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

<aside class="notes" data-markdown>
And what's pizza got to do with anything?
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
</aside>

# PIZZA EFFECTS: IDENTITY

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : attribute-based access control (ABAC)
OpenShift->Kubernetes : roles and cluster-roles
Kubernetes->OpenShift : role-based access control (RBAC)
@enduml
```

# PIZZA EFFECTS: NETWORKING

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : flat network
OpenShift->Kubernetes : multitenant plugin
Kubernetes->OpenShift : network policies
@enduml
```

# PIZZA EFFECTS: GENERIC 

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
Kubernetes->OpenShift : feature unavailable
OpenShift->Kubernetes : imperative API calls
Kubernetes->OpenShift : declarative API objects 
@enduml
```

# NAMESPACES

```render_a2sketch
#=----------------------------#
|                             |
| Cluster                     |
|                             |
| #----------#   #----------# |
| |[p]       |   |[b]       | |
| | Boeing   |   | Airbus   | |
| |          |   |          | |
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
```

# ENABLE ROUTER ACCESS {bg=#6a2469 .light-on-dark}

```render_a2sketch
#=----------------------------#
|[w]                          |
| Cluster                     |
|                             |
| #----------#   #----------# |
| |[p]       |   |[b]       | |
| | Boeing   |   | Airbus   | |
| |          |   |          | |
| |          |   |          | |
| |          |   |          | |
| #---+------#   #-----+----# |
|     ^                ^      |
|     |  #----------#  |      |
|     |  |          |  |      |
|     |  | Router   |  |      |
|     +->+          +<-#      |
|        |          |         |
|        |          |         |
|        #----------#         |
|                             |
#-----------------------------#

[p]: {"a2s:delref": true, "fill": "#ef5ba1", "fillStyle": "solid"}
[b]: {"a2s:delref": true, "fill": "#27bdce", "fillStyle": "solid"}
[w]: {"a2s:delref": true, "fill": "#fff", "fillStyle": "solid"}
```

```
$ oc adm pod-network join-projects --to Router Boeing Airbus
```

<small class="tiny">Projects are a very, very thin wrapper around Kubernetes namespaces.</small>

# WHAT THE PARSER SAW

```{.render_plantuml args="-Sbackgroundcolor=transparent"}
@startuml
scale 550 height
(*) -down-> "NetworkPolicy object found"
note right: let's ignore policies in other namespaces for now
-down-> "Which pods in the namespace are affected by this policy?"
--> "policy type INgress set (or none)?"
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
</aside>

# <small>THANK YOU</small> {bgcss=tw-colorful .light-on-dark}

<small>Slides built with <a href="https://github.com/arnehilmann/markdeck">Markdeck</a><br/>
<i class="fa fa-github" aria-hidden="true"></i> <a href="https://github.com/gerald1248/who-needs-users-slides">gerald1248/who-needs-users-slides</a><br/>
<i class="fa fa-twitter" aria-hidden="true"></i> 03spirit</small>


<img src="assets/images/ThoughtWorks_logo_white.png" alt="ThoughtWorks logo" width="25%"/>