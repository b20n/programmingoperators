Managing the deployment and lifecycle of complex, stateful,
distributed applications is hard work. It's spawned multiple fields -
database administration and site reliability engineering, to name two
- and motivates deeply held architectural opinions. Microservices and
functions-as-a-service wouldn't be what they are today if managing
state was easy.

Kubernetes and similar tools have raised the level of abstraction for
many developers and operators. Programmers today regularly treat
entire datacenters effectively as a single computer. Servers as
"cattle, not pets" is a maxim worn threabare by 2020, but even as
recently as 2005 this would have been an incomprehensible and
unrealizable idea. The future is here... But it is not evenly
distributed. These new, powerful abstractions have been by and large
focused on solving the simpler problems of stateless compute. Stateful
services - databases, caches, message queues, etc. - have been left
behind with their pets. It's not impossible to deploy stateful systes
on Kubernetes - `StatefulSet`, `PersistentVolume`, and other
abstractions are useful building blocks. To properly manage them,
however, to reconfigure, upgrade, scale, or recover from failure,
these systems generally require domain knowledge that simply cannot be
encoded in Kubernetes.

An "operator" is software that implements these higher-level
application-specific behaviors to enable automated management of
complex stateful systems. Operators in Kubernetes utilize the
`CustomResourceDefinition` resource to extend the Kubernetes API in to
the domain of a specific application.

A minimal operator comprises two components: a
`CustomResourceDefinition` (`CRD`) and a controller that acts on
instances of that `CRD`. An instance of a `CRD` is a Kubernetes
resource more or less like any other: it includes standard
`ObjectMeta` fields such as `Name`, `CreationTimestamp`,
`ResourceVersion`, and `Generation` as well as, typically, `Spec` and
`Status` fields whose definitions are up to the implementor.

<< example crd >>

A controller is a procedure that takes an instance of the `CRD` and
optionall performs some action. It might create other resources, make
an API call, reconfigure a database, or launch missiles. Beyond some
very basic limitations, Kubernetes doesn't care.

<< example controller >>

That's it, really. "Operator" may imply big ideas about autonomous,
self-healing distributed systems and thousands of SREs on the streets
looking for jobs, but it doesn't entail them. An operator can be as
big or as small as the role it needs to fill with a system.

Most of the rest of this book is about how to turn this trivial
example in to something worthwhile and useful. There's a lot to cover.
To dip in just a bit deeper let's discuss two big ideas as a slight
enhancement of this simplistic operator.

The first is related to the fact that Operators exist at the behest of
the Kubernetes API, and that API has very particular semantics. One of
these semantics, and perhaps the most important for operator
programmers, is that there is no way to observe every change to a
resource. << EXAMPLE HERE >>. Whatever your operator needs to do, it
neds to do based on the current state of the resource at any given
time. This idea is sometimes called "level-triggering", and it's
essential to writing operators that do what they say on the box. We'll
be talking about it quite a bit.

The second big idea is convergence. Given a resource with a certain
specification, the controller should eventually converge to a stable
state where no more action is taken. "Eventually" could be a very long
time - hundreds of iterations, or hours in the real-world - but the
controller should be working to align the state of the world with the
state specified in the resources it manages. If that specified state
changes - even before the world has been converged - the controller
should start moving towards the new state.

<< tie back to distributed systems >>
