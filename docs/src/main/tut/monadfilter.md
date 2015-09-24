---
layout: default
title:  "MonadFilter"
section: "typeclasses"
source: "https://github.com/non/cats/blob/master/core/src/main/scala/cats/MonadFilter.scala"
scaladoc: "#cats.MonadFilter"
---
# MonadFilter

a MonadFilter is a [`Monad`](monad.html) equipped with an additional method which allows us to
create an "Empty" value for the Monad (for whatever "empty" makes
sense for that particular monad). This is of particular interest to
us since it allows us to add a `filter` method to a Monad, which is
used when pattern matching or using guards in for comprehensions.


```tut
for {
  num <- Option(1) if num == 1
} yield num

for {
  num <- List(1, 3, 4 ,6) if num > 3
} yield num

val result = List(3, 4) match {
  case head :: tail if head == 3 => true
  case _ => false
}

```

### MonadFilter instances

If `Monad` is already present and `flatMap` is well-behaved,
extending the `MonadFunctor` to a `Monad` is trivial. To provide evidence
that a type belongs in the `MonadFilter` typeclass, cats' implementation
requires us to provide an implementation of `empty`.

```tut
import cats._

implicit def optionMonadFilter(implicit app: MonadFilter[Option]) =
  new MonadFilter[Option] {
    // Define empty function
    override def empty[A]: Option[A] = None
  }
```

