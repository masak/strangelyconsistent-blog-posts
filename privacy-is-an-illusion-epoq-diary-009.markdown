---
title: Privacy is an illusion (epoq diary 009)
author: Carl Mäsak
created: 2020-11-03T23:06:00+08:00
---
tl;dr: I like the `private` keyword &mdash; it's my favorite access modifier. But it's not strictly necessary, it's impossible to enforce, and in terms of means to an end, it's arguably not the best one.

This post comes from many small impetuses. The most proximal is this: the Bel language has no `private` access.

Take this Java code, which we'll use as a running example of a small object upholding an invariant:

<pre><code>class Account {
    private int balance = 0;

    public int getBalance() {
        return balance;
    }

    public void deposit(int amount) {
        if (balance + amount &lt; balance) {
            throw new IllegalArgumentException();
        }
        balance += amount;
    }

    public void withdraw(int amount) {
        if (balance - amount &gt; balance) {
            throw new IllegalArgumentException();
        }
        if (balance - amount &lt; 0) {
            throw new OverdraftException();
        }
        balance -= amount;
    }
}</code></pre>

(I tried to be a good citizen and account for signed overflow, although I probably hoist myself by my own petard somehow anyway. Just pretend for the sake of discussion that I got it right.)

An instance of the `Account` class does three public things, via its API: it tells you the current balance, it lets you deposit money into the account, and it lets you withdraw money. Besides making sure you give it non-negative amounts, it also makes sure you cannot withdraw more money than available &mdash; this is the invariant being upheld.

The `Account` implementation also contains a field `balance`, but if you're a client to the class and not an implementor, then &mdash; `private` &mdash; that ain't no business of yours.

Here's as close as I get to implementing the same ideas in Bel:

<pre><code>(tem account balance 0)

(def deposit (acct m)
  (if (&lt; m 0) (err 'negative-amount)
              (++ acct!balance m)))

(def withdraw (acct m)
  (if (&lt; m 0)            (err 'negative-amount)
      (&lt; acct!balance m) (err 'overdraft)
                         (-- acct!balance m)))</code></pre>

Some differences:

* Templates carry data only, so instead of defining methods in a class, we define functions that take an account as a parameter.

* Bel has a notion of types, and there's a way to restrict the `acct` parameter to only account objects, but its more idiomatic not to bother.

* Besides, `account` objects do not know they're account objects. They're just anonymous tables.

* There's no `getAccount` equivalent. Instead, you'd just look up `balance` in an account object.

* The two functions also act directly on `acct!balance`, despite being external to the definition of accounts.

* (Related to the previous two points.) There's no `private`.

The Java world view and the Bel world view are both internally consistent, but rather than try to understand each other, they usually just stare in stunned silence.

I've started to think of programming language design as being inspired by either the _id_ or the _superego_:

* **Id features**: Close to the machine, to machine instructions, physical memory, calling conventions, registers, machine limitations, freedom under responsibility, enough rope to shoot yourself in the foot.

* **Superego features**: Abstract, orthogonal, encapsulated, pure, safe, mathematical, consistent, benign limitations.

From that perspective, Bel's "anything goes" (non-)approach to object privacy seems to be an _id_ feature, and Java's `private` keyword a _superego_ feature. Case closed.

But... not so fast. I would argue that, at the very least, the case is not closed.

## Just to be clear, I like `private`

Before we move on to saying negative things, I would just like to point on that I think `private` fills an actual purpose in Java (and in other languages).

In fact, I think I'd be pretty happy with just `private` and `public` in Java! Never quite took to the other two access levels.

Here's the motivation for `private` in Java: it demarcates the things that are internal from the things that are part of the API. In the very short term, that makes it clear to all the clients of the object what they can and can't access. In the longer term, that translates to the freedom to refactor the internals of the object, with a strong guarantee that this won't affect downstream clients.

## `private` is not strictly necessary

It's interesting to see how many successful languages side with Bel. That's a data point in itself.

Perl and Python; in both these languages, objects are hashes/namespaces where you can get at the attributes directly.

> "Perl doesn't have an infatuation with enforced privacy. It would prefer that you stayed out of its living room because you weren’t invited, not because it has a shotgun." &mdash; Larry Wall

JavaScript doesn't have private access. There's [a tc39 proposal](https://github.com/tc39/proposal-class-fields) to add it (currently stage 3), but that also means the language got to where it is today without it. So, yeah.

Of the three most used languages today &mdash; Java, JavaScript, and Python &mdash; only one of them has `private`. And there's the rub: the other two languages, I would argue, are not cesspools of privacy-thwarting anarchy. Or, if they _are_ (and I didn't notice), then that seems to work for them.

Are those languages struck by a proportionate retribution whenever someone refactors the data representation of their shared object? Again, probably not. This could be either because clients are exceptionally well-behaved, following rules that aren't even there, or because such refactors aren't all that common.

## `private` is impossible to enforce

Back in the day with Raku (née Perl 6), we were discussing the nature of privacy. And it struck me that in the limit, privacy in an object-oriented system is _unenforceable_.

* You could always serialize the object and inspect whatever private data it has. Or you can pause the VM and find the object in memory. Or you can perform a side-channel attack and gain knowledge about the private data of the object. Either way, if you own the machine, the private data is not really private.

* Using the meta-object protocol is equivalent to saying "You know what? I am `root`. Your puny access levels do not apply to me. Your objects are transparent, like water."

Whether from below or from above, your beloved privacy is beleaguered.

And before you counter that No True Scotsman would ever cheat like that, well that's kind of the point &mdash; in the limit, `private` also ends up being a socially-agreed illusion. At most, it's a couple more hoops to jump through, that's all.

## `private` is not the best means to an end

As a final nail in the coffin, it's no longer all that clear to me that objects are the appropriate peg on which to hang the whole privacy thing.

Let's look at some alternatives:

* Long before that tc39 proposal, people had discovered that they could get true privacy via [closures](https://leanpub.com/javascriptallongesix/read#encapsulation). (Scheme aficionados rejoice. As does [venerable master Qc Na](https://people.csail.mit.edu/gregs/ll1-discuss-archive-html/msg03277.html).) This option, by the way, does not work in Bel, because unlike almost all other languages I know, Bel's closures are transparent.

* You can use the `interface`/implementation distinction to hide data. In fact, this would have worked wonders in Java, and maybe would have made people rely on interfaces more? There seems to be a long and uneven struggle between "objects" and "abstract data types" in academia... I have no idea who is winning, or if anyone should. ML's modules/signatures are also a good example of this feature.

* A number of languages (Haskell, Dylan, Go, Rust) control access on the level of packages/modules &mdash; you can see everything in your module, but you control what is visible from the outside. This seems an excellent idea to me. In fact, didn't Java start doing this too with Java 9? How many notions of privacy does one language need? In fact, it's come to my attention that _object-oriented programming_ steamrollered over _modular programming_ sometime around the 1980s, and so this solution might have been more popular if different winners had gotten to write history.

Someone once quipped that the big challenge with object orientation isn't _re-use_, it's _use_. Given how hard it is to get someone else to be a legitimate client to one's object, maybe the most common person whose access one is restricting, is oneself.

Not that that's a bad thing either, _a priori_. 哈哈
