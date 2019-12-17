The rapid growth of Kubernetes <<>>

The purpose of this book is not to discuss "consuming" operators as
part of deploying a software stack on Kubernetes. Operators as a
software delivery mechanism are secondary. Rather, this is about using
the operator pattern to build (and operate!) production-quality
systems.

The ideas underpinning operators are not specific to Kubernetes, but
Kubernetes is where this book's focus is. This is largely a practical
matter: Kubernetes is popular, and has good support for operator-style
systems, and there aren't a lot of folks doing this kind of thing
elsewhere. Regardless, I hope that readeres gain an intuition for
building convergence-based distributed systems that is useful in other
domains. At some point in the future the cool kids won't know what
Kubernetes is, but they'll still need to write correct distributed
systems.

Regarding nomenclature: I'll use the term "operator" (with a
lower-case "o") and "operator pattern" in this book to refer to the
sorts of convergence-based systems discussed throughout. You'll get
the idea. "Operator" is a strange term. Kubernetes generally doesn't
use it: it has a `controller-manager`, not `operator-manager`. But the
word is a useful shorthand. I explicitly don't intend the terms to
refer to RedHat's Operator Framework or Operator Maturity Model,
except where otherwise noted.

Regarding code: this is a book about programming. You should expect to
read code while reading the book, specifically Go code. Go is the
lanugage best supported by the Kubernetes API, which makes it far
easier to build working operators without yak shaving on frameworks.
It's certainly possible to write operators in Python, C, or Bash, but
it's just more work. If you aren't familiar with Go, don't worry -
there's not a whole lot to it, and I don't use any esoteric features
of the language. If you're familiar with any other imperative
language, you'll probably do fine.

Finally, all code in this book should be functional and mostly
idiomatic as of Kubernetes 1.16. Kubernetes moves quickly, and the CRD
interfaces are still evolving as of this writing, but the community
takes backwards-compatibility seriously, so I expect that the gist of
the projects here will continue to function for quite some time.
