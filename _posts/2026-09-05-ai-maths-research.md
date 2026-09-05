---
layout: single
title: "AI, Mathematics, and a Research World Moving Faster"
category: maths
tag: ai
author_profile: false
use_math: true
sidebar:
    nav: "counts"
---

Recently, I saw the news about an AI model disproving a long-standing conjecture related to the **Erdős unit distance problem**.

Then, only a few days ago, I watched a YouTube video by Dr Samuel Allen Alexander discussing the sudden progress on **bounded gaps between primes**, a problem closely connected to the Twin Prime Conjecture.

[Three competing breakthroughs in the last 3 days](http://www.youtube.com/watch?v=6d04h0UqyH0)

Seeing these two things so close together made me think:

> AI has become seriously powerful, and researchers are beginning to squeeze an incredible amount of capability out of these models.

Not necessarily AGI.

Not necessarily a singularity.

But something interesting is definitely happening.

So I wanted to write down some of my recent thoughts about AI, mathematics, research, and where all of this might be going.

## The Erdős result that caught my attention

The Erdős unit distance problem is very easy to explain.

Suppose we place $n$ points on a two-dimensional plane.

How many pairs of those points can be exactly distance 1 apart?

If we call the maximum possible number of unit-distance pairs $u(n)$, the problem is to understand how quickly $u(n)$ can grow as $n$ becomes large.

For decades, the natural constructions were based on lattice-like arrangements.

Erdős conjectured that the growth should essentially remain near-linear:

$ u(n)=n^{1+o(1)}. $

The $o(1)$ term means that the extra amount in the exponent approaches zero as $n$ grows.

This was a very persistent belief.

Then in 2026, an OpenAI reasoning model produced a counterexample.

The new construction shows that for infinitely many values of $n$, there are configurations with at least

$ u(n)\geq n^{1+\delta} $

for some fixed

$ \delta>0. $


That is enough to destroy the old conjecture.

What interested me even more was **how** it happened.

The key ideas came from sophisticated algebraic number theory rather than from the area where I would naturally expect a geometry problem to be attacked.

The proof uses structures related to algebraic number fields and class field towers to construct configurations that beat the behaviour expected from the traditional lattice approach.

That is fascinating.

A researcher working mainly in discrete geometry might spend years developing intuition about geometric configurations.

An AI model does not necessarily have that same psychological attachment to a field boundary.

It can move between mathematical literature from very different areas and try connections that might seem unnatural to a human specialist.

In this case, bringing deep algebraic number theory into an elementary-looking geometry problem happened to work.

That feels like one of the real strengths of current AI.

## But I am not ready to call this AGI

At the same time, I still hesitate when people see results like this and immediately jump to:

> AGI is here.

Current transformer-based models such as GPT and Claude are already extremely strong tools.

For me personally, they have become almost essential.

They save enormous amounts of time when I am programming, reading papers, checking ideas, writing, debugging, studying unfamiliar concepts, or simply trying to understand something faster.

I use them almost every day.

But I still think there is a difference between **extremely capable reasoning inside an existing intellectual world** and a system possessing something like general human intelligence.

The Erdős result is actually a good example of why this distinction is difficult.

It would be unfair to describe the result as simple brute-force search. The model had to make a very non-trivial mathematical connection, construct an infinite family, and produce an argument that human mathematicians could verify.

That is much more impressive than checking a few million candidate configurations.

But it was still working on a clearly defined mathematical target.

The problem already existed.

The definitions already existed.

The standards for what counted as a valid proof already existed.

The mathematical literature containing the relevant tools already existed.

The model found an extraordinary route through that world.

What it did not do was wake up one morning and decide:

> I think mathematics is missing an important concept. I will invent a new language for thinking about it, spend twenty years developing that language, and then reformulate several fields around it.

That kind of conceptual creation still feels different to me.

Maybe future systems will do it.

Maybe sufficiently advanced transformer systems already contain more of that capability than I realise.

Or maybe we will eventually need something architecturally different.

I honestly do not know.

## Maybe transformers are only part of the story

I am personally not convinced that simply scaling today's architecture forever automatically leads to AGI.

It might.

But I would not be surprised if something else is needed.

World models are one interesting direction.

Instead of primarily learning to predict sequences of tokens, a world-model-based system tries to construct internal representations of how environments evolve and how actions produce consequences.

That seems important if we want systems that reason about physical reality rather than only representations of it.

There are also neuro-symbolic approaches that try to combine the flexible pattern recognition of neural networks with the exactness of symbolic reasoning.

And then there are probably architectures we have not invented yet.

Maybe the eventual system will combine:

- neural representation learning
- symbolic verification
- persistent memory
- active experimentation
- world models
- physical embodiment
- autonomous goal formation
- and architectures that currently do not even have names

I find that possibility more interesting than arguing about whether today's model already qualifies as AGI.

For now, these models are incredibly powerful tools.

And researchers are getting extremely good at using them.

## A Computer Science problem I keep coming back to

Thinking about AI solving mathematics naturally brought me back to a problem that feels much closer to home as a Computer Science student:

**P vs NP.**

The basic distinction is beautiful.

Very roughly, \(P\) contains problems that can be **solved** efficiently using a deterministic algorithm.

\(NP\) contains problems where, if someone gives us a candidate solution, that solution can be **verified** efficiently.

The famous question is:

$$
P\stackrel{?}{=}NP.
$$

For example, consider 3-SAT.

We are given Boolean variables and clauses containing three literals.

A formula might look something like:

$$
(x_1\lor \neg x_2\lor x_5)
\land
(\neg x_1\lor x_3\lor x_4)
\land \cdots
$$

If someone gives me a complete assignment of TRUE and FALSE values, checking whether every clause is satisfied is easy.

I simply evaluate the clauses.

But finding such an assignment in the first place can be extremely difficult.

3-SAT is NP-complete.

That means if someone discovered a polynomial-time algorithm that always solved 3-SAT correctly, then every problem in NP could also be solved in polynomial time.

One algorithm would be enough to prove

$$
P=NP.
$$

The opposite direction is much nastier.

To prove

$$
P\neq NP,
$$

it is not enough to invent ten thousand clever polynomial-time SAT algorithms and show that all ten thousand eventually fail.

You somehow need to prove that **no possible polynomial-time algorithm** can solve the problem.

That universal requirement is part of what makes the problem so brutal.

There are even known meta-level barriers showing that broad families of proof techniques are insufficient. Relativisation, Natural Proofs, and algebrisation are examples.

Humanity has not simply failed to find the right proof yet.

We have also discovered reasons why several entire styles of proof cannot settle the problem by themselves.

That is a very different kind of challenge.

## Designing my own terrible polynomial-time SAT solver

Just for fun, I started thinking:

What if I tried to invent a fake polynomial-time algorithm for 3-SAT and watched exactly where it broke?

Suppose the input contains \(n\) variables and \(m\) clauses.

Here is my extremely suspicious algorithm.

### Step 1

Start with every variable unassigned.

For intuition, imagine representing an undecided Boolean variable as

$$
x_i=0.5.
$$

This is not literally a Boolean assignment. It is just a way of saying:

> I have not committed to TRUE or FALSE yet.

### Step 2

For each remaining variable, estimate how good the formula would look if that variable were set to TRUE versus FALSE.

For a clause whose unresolved literals are treated as independent 50/50 possibilities, I can even invent a temporary "satisfaction score".

For example, if all three literals are still undecided, the probability that the clause fails is

$$
\left(\frac12\right)^3=\frac18,
$$

so the probability that it is satisfied is

$$
1-\frac18=\frac78.
$$

### Step 3

Measure how much the overall score changes when each variable becomes TRUE or FALSE.

### Step 4

Choose the variable and value that produces the biggest immediate improvement.

Commit to it permanently.

### Step 5

Run unit propagation and simplify the formula.

### Step 6

Repeat until everything has been assigned.

This sounds surprisingly reasonable.

And the runtime can clearly be kept polynomial.

A naive implementation that reevaluates every remaining variable against every clause at each stage might take roughly

$$
O(n^2m),
$$

which is still polynomial.

Amazing.

I have solved P vs NP.

Time to collect the Millennium Prize.

Unfortunately, there is a minor problem.

The algorithm is wrong.

## The wall: local decisions are not global solutions

The greedy score only tells me which choice looks good **right now**.

It does not tell me whether that choice destroys the only satisfying assignment somewhere much deeper in the search space.

Imagine that setting \(A=\text{TRUE}\) immediately helps satisfy 99 clauses.

Great.

So my algorithm commits to TRUE.

Much later, perhaps after dozens of other assignments, I discover a small group of clauses whose only jointly consistent solution required

$$
A=\text{FALSE}.
$$

The original formula may have been satisfiable.

I simply walked into the wrong region of the search space.

The obvious response is:

> Fine. Go back and change A.

And there it is.

Backtracking.

Once I allow the algorithm to repeatedly undo earlier decisions and explore alternative branches, the search tree can explode.

In the worst case, the number of possible Boolean assignments is

$$
2^n.
$$

Suddenly my beautiful polynomial-time algorithm is staring at exponential complexity again.

Modern SAT solvers are obviously far more sophisticated than this silly greedy algorithm. They use techniques such as conflict-driven clause learning, clever branching heuristics, propagation, restarts, preprocessing, and many other optimisations.

They can solve enormous practical instances.

But worst-case complexity is still the monster hiding underneath everything.

Trying to design even a fake polynomial SAT algorithm made me appreciate the problem more.

There is this frustrating choice:

- commit aggressively and risk being wrong
- explore alternatives and risk exponential growth

Of course, proving that **every conceivable algorithm** must eventually face something equivalent to this problem is exactly the part nobody knows how to do.

## Then I wondered: what about quantum mechanics?

At this point my brain naturally went:

> What if we escape the local optimum without normal backtracking?

This is where quantum computing becomes really interesting.

One approach is **quantum annealing**.

A combinatorial optimisation problem can sometimes be encoded into an energy landscape.

Boolean variables can be represented through binary variables or spins, and the objective is designed so that a valid or optimal solution corresponds to a low-energy state.

In an Ising-style representation, a cost function might look roughly like

$$
H(s)
=
\sum_i h_i s_i
+
\sum_{i<j}J_{ij}s_is_j,
$$

where

$$
s_i\in\{-1,+1\}.
$$

The goal becomes finding the ground state:

$$
s^\ast=\arg\min_s H(s).
$$

Classically, a complicated energy landscape can contain many local minima.

A thermal process may need enough energy to climb over a barrier.

Quantum systems introduce another possibility: tunnelling.

Instead of literally climbing over a narrow barrier, quantum amplitude can pass through it.

That sounds almost like exactly the cheat code I wanted.

But unfortunately, nature does not hand us P = NP that easily.

## Quantum computers are powerful, but not magic

Quantum computing changes what efficient computation can look like.

The complexity class usually associated with efficient quantum computation is **BQP**.

We know, for example, that

$$
P\subseteq BQP.
$$

Quantum computers can simulate ordinary polynomial-time classical computation.

And BQP itself sits inside larger classical complexity classes; one useful known containment is

$$
BQP\subseteq PP\subseteq PSPACE.
$$

The interesting question is where NP fits.

We do **not** know whether NP is contained in BQP.

That is important.

It has not been proven that quantum computers cannot solve NP-complete problems efficiently.

But most complexity researchers do not expect a generic quantum computer to turn all NP-complete problems into polynomial-time problems.

Even the famous quantum speedups illustrate why.

Suppose I have an unstructured search space containing

$$
N=2^n
$$

possible assignments.

Classically, brute-force search may require on the order of

$$
O(2^n)
$$

checks.

Grover's quantum search reduces this to roughly

$$
O(\sqrt{2^n})
=
O(2^{n/2}).
$$

That is a huge improvement.

But it is still exponential.

It does not magically become

$$
O(n^k).
$$

Quantum annealing has similar caveats.

Tunnelling can help with certain landscapes, especially barriers that are narrow in an appropriate sense.

But an optimisation landscape can also contain extremely difficult structures, and in adiabatic approaches the minimum spectral gap can become very small.

When that happens, the runtime required to remain near the desired ground state can become enormous.

So the quantum computer has not obviously escaped complexity.

It has changed the geometry of the search.

That is still incredibly useful.

But the wall is still there.

## Is computational complexity somehow a law of nature?

This led me to a more philosophical question.

What if P vs NP is not just difficult because humans have not invented the right technology yet?

What if computational complexity reflects something deeper about how information can exist and be processed in the universe?

I find that thought strangely appealing.

The speed of light gives us a physical limit on how quickly information can propagate through space.

Maybe complexity gives us another kind of limit: not on how fast information travels, but on how efficiently certain structures can be discovered from incomplete information.

But I have to be careful here.

This is an analogy, not a theorem.

Nothing in thermodynamics currently proves

$$
P\neq NP.
$$

Quantum mechanics does not prove it either.

And P vs NP is a mathematical question about abstract computational models, not directly a statement about the hardware available in our universe.

Still, I find the analogy interesting.

Physics repeatedly gives us situations where something that looks like an engineering limitation eventually turns out to be a fundamental constraint.

Maybe complexity contains limits of that kind.

Or maybe one day someone will discover an algorithm that completely destroys this intuition and proves

$$
P=NP.
$$

That would be hilarious.

And slightly terrifying.

## Then the prime-gap frontier started moving

While I was already thinking about all of this, another story appeared.

For years, one important number associated with bounded gaps between primes had been **246**.

If \(p_n\) denotes the \(n\)-th prime, define

$$
H_1
=
\liminf_{n\rightarrow\infty}
(p_{n+1}-p_n).
$$

The Twin Prime Conjecture would imply

$$
H_1=2.
$$

We are nowhere near proving that.

But the major breakthrough beginning with Yitang Zhang, followed by work from James Maynard, Terence Tao, Polymath and others, established that \(H_1\) is at least finite.

Eventually the unconditional published frontier reached

$$
H_1\leq246,
$$

where it remained for years.

Then suddenly, at the end of August and beginning of September 2026, everything started moving again.

Julia Stadlmann posted a new result showing

$$
H_1\leq240.
$$

Soon afterwards, another result pushed the bound to 212.

Then OpenAI published work targeting

$$
H_1\leq186,
$$

together with a Lean formalisation and numerical certificate.

Watching a frontier that had been sitting at 246 for roughly a decade suddenly move

$$
246\rightarrow240\rightarrow212\rightarrow186
$$

within days was wild.

This does **not** solve the Twin Prime Conjecture.

We are still very far from 2.

But that is almost what made it more interesting to me.

A long-standing frontier had suddenly become active again.

## AI is very good when there is something to push against

This brought me back to my earlier thought about the Erdős result.

A lot of recent AI-assisted mathematical progress seems to happen where humans have already built an enormous amount of structure.

The theory exists.

The machinery exists.

There are inequalities to optimise, parameter spaces to search, candidate constructions to test, symbolic arguments to manipulate, numerical certificates to verify, and formal systems such as Lean that can help check parts of the reasoning.

Humans have built the map.

AI can now explore that map with an absurd amount of persistence.

That does not make the achievement trivial.

If anything, the Erdős result demonstrates that the exploration can be deep enough to cross boundaries between areas of mathematics that humans had not connected in the right way.

But I still see a possible distinction between:

> **searching an existing intellectual space extremely well**

and

> **inventing a fundamentally new intellectual space**

The boundary between those two things is obviously blurry.

Human mathematicians also build new theories out of old ideas.

Nobody creates mathematics from literally nothing.

But some breakthroughs feel like a new optimisation inside an existing framework, while others change the language in which future problems are even expressed.

That second category still feels much harder.

## Why problems like P vs NP feel different to me

This is one reason P vs NP fascinates me.

If

$$
P=NP,
$$

then in principle one successful polynomial-time algorithm for an NP-complete problem such as SAT would settle the question.

That direction has something of an existential flavour.

Find the algorithm.

Verify the runtime.

Done.

But if

$$
P\neq NP,
$$

then we need a genuine lower bound.

We need to rule out an entire universe of possible polynomial-time algorithms.

And complexity theory has already shown that several extremely natural proof strategies cannot be enough.

This is why simply throwing more search at the problem may not work.

There may not be a useful parameter space where the objective is simply:

> minimise this value until it becomes zero.

The missing object may be a new proof idea.

Maybe AI will eventually invent it.

I would certainly not bet against AI after what has happened recently.

But I also would not assume that making today's reasoning model ten times larger automatically generates it.

## The Riemann Hypothesis gives me a similar feeling

I have a similar intuition when thinking about the Riemann Hypothesis.

The hypothesis says that every non-trivial zero of the Riemann zeta function

$$
\zeta(s)
$$

has real part

$$
\operatorname{Re}(s)=\frac12.
$$

Computers can verify huge numbers of zeros.

AI can search identities, inspect literature, test constructions, manipulate symbolic expressions, and help mathematicians explore different approaches.

But checking another trillion zeros does not prove the theorem.

The statement is universal.

It concerns all non-trivial zeros.

And unlike a finite optimisation problem, there is no obvious search objective where finding one particularly good set of parameters automatically gives us the proof.

There are deep programs such as the Hilbert-Pólya idea, which suggests that the imaginary parts of the zeros might correspond to eigenvalues of some self-adjoint operator.

Maybe some future connection through spectral theory, geometry, physics, or another field will provide the missing structure.

Maybe AI will help discover that structure.

But I think I should be careful about saying that the Riemann Hypothesis has formal "barrier theorems" in the same sense as P vs NP.

P vs NP genuinely has results such as relativisation, Natural Proofs, and algebrisation showing limitations of broad proof techniques.

The difficulty of the Riemann Hypothesis is different.

Many approaches have reached serious technical obstacles, but we do not have an equivalent theorem saying that all methods of some enormous general class cannot possibly prove RH.

So the analogy is useful to me, but it is not exact.

## Maybe the interesting thing is not AI replacing mathematicians

The more I think about it, the less interesting the question

> Will AI replace mathematicians?

feels.

A more interesting question is:

> What can mathematicians, scientists, and engineers discover when the cost of exploring ideas becomes dramatically lower?

Suppose I have ten plausible approaches.

Historically, exploring each one seriously might require days, weeks, or months.

Now I can use AI to:

- search related literature
- generate candidate derivations
- translate an argument into formal notation
- test edge cases
- write experimental code
- run numerical searches
- compare several mathematical approaches
- identify known counterexamples
- explain unfamiliar theory
- and act as a critic of my own reasoning

Most approaches will still fail.

But failure becomes cheaper.

That matters.

If I can explore one hundred serious ideas instead of ten, perhaps one of them reaches somewhere unexpected.

That is the part of AI-assisted research that excites me the most.

Not replacing the researcher.

Increasing the number of intellectual paths the researcher can afford to explore.

## Research suddenly feels very fast

This feeling is not limited to mathematics.

I am experiencing a smaller version of it directly during my Honours year.

My 2026 research is on **3D coronary artery segmentation from coronary CT angiography**.

Earlier this year, I was working with the ImageCAS dataset and reading the existing papers around it.

Then on **31 August 2026**, a new paper appeared on arXiv:

**ImageCAS-X: a dataset and benchmark for coronary artery segmentation and centerline extraction in coronary CT angiography.**

It is effectively a major follow-up around the same dataset family I have been working with, introducing additional annotations and benchmarking for hundreds of scans.

My reaction was basically:

> Oh. Another one just appeared.

Of course this is normal in research.

New papers appear all the time.

But experiencing it while doing my own Honours project feels very different from hearing about it in a lecture.

When you are a student reading a textbook, knowledge feels static.

Chapter 3 exists.

Then Chapter 4 exists.

Someone already figured everything out and organised it nicely.

Research does not feel like that at all.

I can read the current literature, design an experiment, start implementing something, and then suddenly a new preprint appears that changes what is available.

Someone somewhere else was working on a related problem at exactly the same time.

I did not know who they were.

They did not know who I was.

There might be ten other groups doing something similar.

Or one hundred.

It feels like running in a race at night where you cannot see the other runners.

You only occasionally see a new paper appear ahead of you and realise:

> Ah. Someone was there.

That is a very different feeling.

## How do people do a four-year PhD now?

This honestly makes me wonder what doing a PhD over four years feels like in the current research environment.

Four years is a long time.

AI capabilities can change dramatically in four months.

A dataset can appear.

A benchmark can change.

A new architecture can suddenly become standard.

Someone can publish a method that makes half of your original plan obsolete.

A research frontier that sat still for ten years can apparently move several times in one week.

How do you choose a research question that remains interesting for four years?

I assume the answer is that a good PhD cannot simply be:

> I will implement technique X on dataset Y.

It probably needs to develop deeper understanding.

Methods change.

Models change.

Datasets change.

But good questions survive longer.

That is another reason I am increasingly attracted to research rather than simply chasing whatever model or framework is currently popular.

## I think choosing research was the right direction for me

Despite all this uncertainty, I actually feel more confident about the direction I have taken.

I am glad I chose to continue into Honours.

Research is difficult.

Sometimes experiments do not work.

Sometimes the literature makes me realise that an idea is not as original as I thought.

Sometimes I spend hours trying to understand why one metric moved by 0.01.

But there is something extremely satisfying about working near the boundary of what is currently known.

There is no answer sheet.

That makes it uncomfortable.

It also makes it meaningful.

This coming summer, I have another opportunity I am really looking forward to.

I received an offer for a studentship with **CSIRO Space & Astronomy**, where I will be working on research involving black-hole jet images.

A year ago, I would not have predicted that I would move from an AI/HPC computational chemistry internship, into coronary artery segmentation for Honours, and then toward astronomy imaging.

But somehow the connecting thread makes sense to me.

AI.

Computer vision.

Scientific computing.

Research.

Using computation to extract information from things that are difficult for humans to analyse directly.

I like that direction.

## I am glad I built the Computer Science foundations first

Looking back, I am also glad that I spent the last few years building the Computer Science foundations underneath all of this.

Programming.

Algorithms.

Data structures.

Machine learning.

Computer vision.

High-performance computing.

Linux.

Software engineering.

Mathematics.

All of these things sometimes felt like separate subjects while I was studying them.

Now they are starting to connect.

AI makes it much easier to enter unfamiliar areas quickly, but I still think the foundations matter.

If the model gives me an algorithm, I need enough knowledge to question its complexity.

If it gives me a proof, I need enough mathematics to notice whether a step is unjustified.

If it generates code, I need enough software knowledge to know whether the architecture makes sense.

If it analyses scientific data, I need enough domain understanding to know whether the result is meaningful.

The stronger the AI becomes, the more important judgement seems to become.

That is an interesting paradox.

## I would love to use AI to find a real breakthrough someday

I do not expect to solve P vs NP next weekend.

Probably a good idea.

But I genuinely would like to keep experimenting with AI-assisted research as side projects.

Not another wrapper around an API.

Not generating random content.

Something where there is an actual problem, an objective, a dataset, a theorem, an experiment, or some measurable unknown.

Maybe computer vision.

Maybe scientific computing.

Maybe optimisation.

Maybe mathematics.

Maybe something I have not encountered yet.

The recent mathematical results made this feel much less ridiculous than it would have sounded a few years ago.

A student with a computer, access to strong reasoning models, open research papers, open-source tools, datasets, formal systems, and enough curiosity now has an extraordinary amount of intellectual leverage.

Most experiments will go nowhere.

That is fine.

One interesting result is enough.

## What happens when AI can do the physical work too?

There is still a much bigger question sitting behind all of this.

If the next ten or fifteen years bring capable robots together with increasingly general AI systems, then automation will no longer be limited to information work.

Software work can be automated.

Writing can be automated.

Analysis can be automated.

Then perhaps a large amount of physical work becomes automated too.

At some point, that creates a strange question:

> If machines can do almost everything useful, what are humans actually supposed to do?

I do not have a good answer.

Maybe family.

Maybe relationships.

Maybe art.

Maybe sport.

Maybe exploration.

Maybe simply enjoying life.

But personally, research is one of the areas that still feels deeply motivating.

There is always another question.

Another thing we do not understand.

Another observation that does not fit.

Another experiment to run.

Another strange connection between fields that nobody has tried yet.

Even if AI becomes dramatically more capable, I think I would still want to participate in that process.

Maybe the role of a researcher changes from doing every calculation manually to deciding which questions are worth asking, designing experiments, connecting domains, testing claims, and directing increasingly capable computational collaborators.

Or maybe AI eventually becomes better at those things too.

Who knows.

For now, I am just enjoying watching the frontier move.

## Anyway, back to my own research

The Erdős result surprised me.

The prime-gap progress surprised me again.

Then ImageCAS-X appeared almost exactly in the research area I am working on.

It all gave me the same feeling:

**research is moving incredibly fast.**

Sometimes that is intimidating.

It feels like an invisible race where new competitors and new tools appear without warning.

But it is also exciting.

I have spent the last few years studying Computer Science because I wanted to work with AI and contribute to something meaningful with it.

Now I finally feel like I am getting closer to the part where I can actually try.

For the moment, my goal is much smaller than solving an 80-year-old mathematical problem.

Finish my Honours year well.

Do good research.

Learn as much as I can.

Make the most of the CSIRO studentship this summer.

And keep building enough mathematical, computational, and scientific understanding that one day I might be able to use these tools to find something genuinely new.

That would be pretty cool.