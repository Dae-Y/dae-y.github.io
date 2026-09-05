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

A few days later, I watched a YouTube video by Dr Samuel Allen Alexander discussing the sudden progress on **bounded gaps between primes**, a problem closely connected to the Twin Prime Conjecture.

[Three competing breakthroughs in the last 3 days](http://www.youtube.com/watch?v=6d04h0UqyH0)

Seeing these results so close together made me think about how powerful AI has become as a research tool. Researchers are pushing current LLMs and reasoning models close to their limits, almost to the point where using them well feels like an art in itself.

I still do not think this means AGI or a singularity has arrived. Current transformer-based systems such as GPT and Claude are incredibly strong tools and have become essential time-savers in my own life, but I still suspect that something more than scaling the same architecture may be needed for truly general intelligence.

What interests me more right now is how useful these systems are becoming for actual research.

## The Erdős result that caught my attention

The Erdős unit distance problem is easy to state.

Suppose we place $n$ points on a two-dimensional plane. How many pairs of those points can be exactly distance 1 apart?

If we call the maximum possible number of unit-distance pairs $u(n)$, the problem is to understand how quickly $u(n)$ can grow as $n$ becomes large.

For decades, the natural constructions were based on lattice-like arrangements. Erdős conjectured that the growth should essentially remain near-linear, written as $u(n)=n^{1+o(1)}$.

The $o(1)$ term means that the extra amount in the exponent approaches zero as $n$ grows.

Then in 2026, an OpenAI reasoning model produced a counterexample. The new construction shows that for infinitely many values of $n$, there are configurations with at least $u(n)\geq n^{1+\delta}$ for some fixed $\delta>0$.

That is enough to disprove the old conjecture.

What interested me even more was **how** the result was found. The key ideas came from sophisticated algebraic number theory rather than from the area where I would naturally expect a geometry problem to be attacked. The proof uses structures related to algebraic number fields and class field towers to construct configurations that beat the behaviour expected from traditional lattice approaches.

A researcher working mainly in discrete geometry may naturally develop intuition around geometric constructions. AI-assisted exploration is not tied to a researcher's disciplinary habits in quite the same way, so it can sometimes surface connections across fields that may not be immediately obvious.

In this case, bringing deep algebraic number theory into an elementary-looking geometry problem worked.

That feels like one of the strongest aspects of current AI: not necessarily inventing mathematics from nothing, but exploring a huge existing intellectual space without being as constrained by human habits or disciplinary boundaries.

## But I am not ready to call this AGI

It would be unfair to describe the Erdős result as simple brute-force search. The result involved a non-trivial mathematical connection, an infinite construction, and an argument that human mathematicians could verify.

At the same time, the model was still working on a clearly defined mathematical target. The problem already existed, the definitions were known, the standards for a valid proof were established, and the relevant mathematical literature already existed.

The model found an extraordinary route through that world.

What still feels different to me is **conceptual creation**: deciding that mathematics is missing an important new concept, inventing a new language or framework, and then reorganising future work around it.

Human mathematicians such as Grothendieck did not simply optimise within an existing framework. They helped create new ways of thinking about entire areas of mathematics.

Maybe future AI systems will do something similar. Maybe advanced transformer systems already contain more of that capability than I realise. Or maybe a deeper architectural change will be needed.

I do not know yet, and that is part of what makes this period interesting.

## Maybe transformers are only part of the story

I am not convinced that simply scaling today's transformer architecture forever automatically leads to AGI.

World models are one interesting direction. Instead of mainly learning to predict token sequences, a world-model-based system tries to build internal representations of how environments evolve and how actions produce consequences.

Neuro-symbolic approaches are also interesting because they attempt to combine the flexible pattern recognition of neural networks with the exactness of symbolic reasoning.

Maybe future systems will combine world models, symbolic reasoning, memory, embodiment, or something we have not invented yet.

For now, I find it more useful to think of current models as extremely powerful research tools rather than argue over whether they already count as AGI.

## A Computer Science problem I keep coming back to

Thinking about AI solving mathematics naturally brought me back to a problem that feels much closer to home as a Computer Science student: **P vs NP**.

Very roughly, $P$ contains problems that can be **solved** efficiently using a deterministic algorithm, while $NP$ contains problems where a proposed solution can be **verified** efficiently.

The famous question is $P\stackrel{?}{=}NP$.

Consider 3-SAT. We are given Boolean variables and clauses containing three literals. A formula might look like $(x_1\lor \neg x_2\lor x_5)\land(\neg x_1\lor x_3\lor x_4)\land\cdots$.

If someone gives me a complete assignment of TRUE and FALSE values, checking whether every clause is satisfied is easy. Finding such an assignment in the first place can be much harder.

3-SAT is NP-complete. If someone discovered a polynomial-time algorithm that always solved 3-SAT correctly, then every problem in NP could also be solved in polynomial time. One such algorithm would be enough to prove $P=NP$.

The opposite direction is much nastier. To prove $P\neq NP$, it is not enough to invent ten thousand clever polynomial-time SAT algorithms and show that all ten thousand eventually fail. You somehow need to prove that **no possible polynomial-time algorithm** can solve the problem.

That universal requirement is one reason the problem is so difficult.

There are also known barriers showing that broad families of proof techniques are insufficient. Relativisation, Natural Proofs, and algebrisation are examples. Humanity has not only failed to find the right proof; we have also discovered reasons why several major styles of proof cannot settle the problem by themselves.

## Playing with a polynomial-time SAT idea

With some help from AI, I played around with a toy solver inspired by a known idea from **MAX-3SAT**: treat unassigned variables as 50/50 possibilities, estimate the expected number of satisfied clauses, and greedily choose the better assignment.

For a 3-literal clause whose variables are still unresolved, the probability that all three literals are false is $\left(\frac{1}{2}\right)^3=\frac{1}{8}$, so a random assignment satisfies the clause with probability $1-\frac{1}{8}=\frac{7}{8}$.

This leads to the well-known conditional-expectation idea behind a polynomial-time $7/8$ approximation for MAX-3SAT.

A simplified version looks like this:

**Step 1:** Start with every variable unassigned.

**Step 2:** For each remaining variable, compare the expected number of satisfied clauses if it is set to TRUE or FALSE.

**Step 3:** Choose the better option, fix that variable, and simplify the formula.

**Step 4:** Repeat until every variable has been assigned.

A straightforward implementation can be kept polynomial, although the exact runtime depends on how the scores are recomputed.

So have we solved P vs NP and earned the Millennium Prize? Unfortunately, no.

The key difference is that this kind of method is an **approximation algorithm for MAX-3SAT**, not an exact polynomial-time solver for 3-SAT. It can guarantee a good assignment, but it does not guarantee that every clause will be satisfied even when a satisfying assignment exists.

## The wall: local decisions are not global solutions

The problem becomes clearer if I think of the method as a greedy SAT solver.

A choice can look very good locally while still destroying the only fully satisfying assignment deeper in the search space. Setting $A=\text{TRUE}$ might immediately satisfy many clauses, but later I could discover that the remaining formula was satisfiable only if $A=\text{FALSE}$.

The obvious response is to go back and try the other choice.

And there it is: backtracking.

Once the algorithm starts undoing earlier decisions and exploring alternative branches, the search tree can grow exponentially. In the worst case, there are $2^n$ possible Boolean assignments.

Modern SAT solvers are far more sophisticated than this toy example. They use techniques such as conflict-driven clause learning, propagation, branching heuristics, restarts, and preprocessing, and they solve very large practical instances extremely well.

But the worst-case complexity problem remains.

Following this simple example helped me see the distinction more clearly: finding a polynomial-time approximation is one thing; finding an exact polynomial-time algorithm for an NP-complete problem is something entirely different.

And proving that **no possible polynomial-time algorithm** can ever do it is harder again.

## Then I wondered: what about quantum mechanics?

At this point my brain naturally went to another question:

> What if we escape the local optimum without normal backtracking?

This is where quantum computing becomes really interesting.

One approach is **quantum annealing**. A combinatorial optimisation problem can sometimes be encoded into an energy landscape, where Boolean variables are represented through binary variables or spins and the objective is designed so that a valid or optimal solution corresponds to a low-energy state.

In an Ising-style representation, a cost function might look like $H(s)=\sum_i h_i s_i+\sum_{i<j}J_{ij}s_is_j$, where $s_i\in\{-1,+1\}$.

The goal becomes finding the ground state $s^\ast=\arg\min_s H(s)$.

Classically, a complicated energy landscape can contain many local minima. A thermal process may need enough energy to climb over a barrier. Quantum systems introduce another possibility: tunnelling.

Instead of literally climbing over a narrow barrier, quantum amplitude can pass through it.

That sounds almost like the cheat code I wanted.

But unfortunately, nature does not hand us $P=NP$ that easily.

## Quantum computers are powerful, but not magic

The complexity class usually associated with efficient quantum computation is **BQP**.

We know that $P\subseteq BQP$, because quantum computers can simulate ordinary polynomial-time classical computation. BQP also sits inside larger classical complexity classes; one useful known containment is $BQP\subseteq PP\subseteq PSPACE$.

The interesting question is where NP fits.

We do **not** know whether NP is contained in BQP. It has not been proven that quantum computers cannot solve NP-complete problems efficiently, but most complexity researchers do not expect a generic quantum computer to turn all NP-complete problems into polynomial-time problems.

The famous quantum speedups illustrate why. Suppose I have an unstructured search space containing $N=2^n$ possible assignments. Classically, brute-force search may require around $O(2^n)$ checks. Grover's quantum search reduces this to roughly $O(\sqrt{2^n})=O(2^{n/2})$.

That is a huge improvement, but it is still exponential. It does not magically become $O(n^k)$.

Quantum annealing has similar caveats. Tunnelling can help with certain energy landscapes, especially when barriers are narrow in an appropriate sense, but some landscapes remain extremely difficult. In adiabatic approaches, the minimum spectral gap can become very small, causing the required runtime to grow dramatically.

So quantum computing changes the geometry of the search, but it has not obviously escaped computational complexity.

## Is computational complexity somehow a law of nature?

This led me to a more philosophical question.

What if P vs NP is not difficult only because humans have not invented the right technology yet? What if computational complexity reflects something deeper about how information can be processed in the universe?

I like the analogy with the speed of light. The speed of light limits how quickly information can propagate through space. Maybe complexity gives us another kind of limit: not on how fast information travels, but on how efficiently certain structures can be discovered from incomplete information.

Still, this is only an analogy, not a theorem. Nothing in thermodynamics proves $P\neq NP$, and quantum mechanics does not prove it either. P vs NP is a mathematical question about abstract computational models, not directly a statement about the hardware available in our universe.

Maybe complexity really does contain fundamental limits of this kind. Or maybe someone will eventually discover an algorithm that completely destroys this intuition and proves $P=NP$.

That would be both hilarious and slightly terrifying.

## Then the prime-gap frontier started moving

While I was already thinking about all of this, another story appeared.

For years, one important number associated with bounded gaps between primes had been **246**.

If $p_n$ denotes the $n$-th prime, define $H_1=\liminf_{n\rightarrow\infty}(p_{n+1}-p_n)$.

The Twin Prime Conjecture would imply $H_1=2$.

We are nowhere near proving that, but the major breakthrough beginning with Yitang Zhang, followed by work from James Maynard, Terence Tao, Polymath and others, established that $H_1$ is finite.

Eventually the unconditional published frontier reached $H_1\leq246$, where it remained for years.

Then, at the end of August and beginning of September 2026, the area suddenly became very active again. Stadlmann posted a new result showing $H_1\leq240$, followed by further AI-assisted claims and formalisation work proposing bounds of 212 and 186.

Watching a frontier that had sat at 246 for roughly a decade suddenly produce new results and claims around 240, 212, and 186 within days was wild.

This does **not** solve the Twin Prime Conjecture. We are still very far from 2. But that is almost what made it more interesting: a long-standing frontier had suddenly become active again.

## AI is very good when there is something to push against

This brought me back to the Erdős result.

A lot of recent AI-assisted mathematical progress seems to happen where humans have already built a large amount of structure. The theory exists, the machinery exists, and there are inequalities to optimise, parameter spaces to search, candidate constructions to test, symbolic arguments to manipulate, numerical certificates to verify, and formal systems such as Lean that can help check the reasoning.

Humans have built the map, and AI can now explore that map with enormous persistence.

That does not make the achievement trivial. The Erdős result shows that the exploration can be deep enough to cross boundaries between mathematical fields that humans had not connected in the right way.

Still, I see a possible distinction between **searching an existing intellectual space extremely well** and **inventing a fundamentally new intellectual space**.

The boundary is obviously blurry. Human mathematicians also create new theories by recombining old ideas. But some breakthroughs mainly optimise within an established framework, while others change the language in which future problems are expressed.

That second category still feels much harder.

## Why problems like P vs NP feel different to me

This is one reason P vs NP fascinates me.

If $P=NP$, then one successful polynomial-time algorithm for an NP-complete problem such as SAT would settle the question. Find the algorithm, verify the runtime, and the proof is done.

If $P\neq NP$, however, we need a genuine lower bound. We need to rule out an entire universe of possible polynomial-time algorithms, and complexity theory has already shown that several natural proof strategies are not enough.

That makes simple large-scale search less obviously useful. There may not be a clean parameter space where the goal is simply to minimise some value until it reaches zero. The missing object may be a genuinely new proof idea.

Maybe AI will eventually invent it. I would certainly not bet against AI after some of the recent results, but I also would not assume that making today's reasoning model ten times larger automatically gives us the answer.

## The Riemann Hypothesis gives me a similar feeling

I have a similar intuition when thinking about the Riemann Hypothesis.

The hypothesis says that every non-trivial zero of the Riemann zeta function $\zeta(s)$ has real part $\operatorname{Re}(s)=\frac12$.

Computers can verify huge numbers of zeros, and AI can search identities, inspect literature, test constructions, manipulate symbolic expressions, and help mathematicians explore different approaches.

But checking another trillion zeros does not prove the theorem. The statement concerns **all** non-trivial zeros.

Unlike a finite optimisation problem, there is no obvious search objective where finding one particularly good set of parameters automatically gives us the proof.

Ideas such as the Hilbert-Pólya program suggest that the imaginary parts of the zeros might correspond to eigenvalues of some self-adjoint operator. Maybe some future connection through spectral theory, geometry, physics, or another field will provide the missing structure, and maybe AI will help discover it.

I also think it is important not to overstate the analogy with P vs NP. P vs NP has formal barrier results such as relativisation, Natural Proofs, and algebrisation. The Riemann Hypothesis has many deep technical obstacles, but not an equivalent theorem saying that entire broad classes of proof methods cannot possibly work.

So the problems feel similar to me in one sense, but they are not structurally identical.

## Maybe the interesting thing is not AI replacing mathematicians

The more I think about it, the less interesting the question "Will AI replace mathematicians?" feels.

A better question is: **what can mathematicians, scientists, and engineers discover when the cost of exploring ideas becomes dramatically lower?**

AI can already help me search literature, test ideas, write experimental code, check edge cases, and explore directions much faster than before.

Most ideas will still fail, but failure becomes cheaper.

If a researcher can seriously explore one hundred ideas instead of ten, perhaps one of them reaches somewhere unexpected. That is the part of AI-assisted research that excites me most: not replacing the researcher, but increasing the number of intellectual paths the researcher can afford to explore.

## Research suddenly feels very fast

This feeling is not limited to mathematics.

I am experiencing a smaller version of it directly during my Honours year.

My 2026 research is on **3D coronary artery segmentation from coronary CT angiography**. Earlier this year, I was working with the ImageCAS dataset and reading the existing papers around it.

Then on **31 August 2026**, a new paper appeared on arXiv:

**ImageCAS-X: a dataset and benchmark for coronary artery segmentation and centerline extraction in coronary CT angiography.**

It is a major follow-up around the same dataset family I have been working with, introducing additional annotations and benchmarking for hundreds of scans.

New papers appear all the time, of course, but experiencing this while doing my own research feels very different from hearing about it in a lecture.

When reading a textbook, knowledge feels static. Everything has already been organised into chapters and sections. Research is much messier. I can read the current literature, design an experiment, start implementing something, and then suddenly a new preprint appears that changes what is available.

Someone somewhere else may have been working on a related problem at exactly the same time, and I had no idea they existed until their paper appeared.

It feels like running in a race at night where you cannot see the other runners. Every now and then, a new paper appears and reminds you that someone else was moving in the same direction.

## How do people do a four-year PhD now?

This makes me wonder what doing a four-year PhD feels like in the current research environment.

Four years is a long time. AI capabilities can change dramatically in a few months. A new dataset can appear, a benchmark can change, a new architecture can become standard, or someone can publish a method that makes part of your original plan obsolete.

A research frontier that sat still for ten years can apparently move several times in one week.

I assume this is why a strong PhD cannot simply be "I will implement technique X on dataset Y". Methods, models, and datasets change, but good research questions can survive much longer.

That is another reason I am increasingly attracted to research rather than simply chasing whichever model or framework is popular at the moment.

## I think choosing research was the right direction for me

Despite all this uncertainty, I feel more confident about the direction I have taken.

I am glad I chose to continue into Honours.

Research is difficult. Experiments fail, papers sometimes reveal that an idea is less original than I hoped, and I can spend hours trying to understand why one metric moved by 0.01.

But there is something satisfying about working near the boundary of what is currently known. There is no answer sheet, which makes the work uncomfortable at times, but also meaningful.

This coming summer, I have another opportunity I am really looking forward to. I received an offer for a studentship with **CSIRO Space & Astronomy**, where I will be working on research involving black-hole jet images.

A year ago, I would not have predicted that I would move from an AI/HPC computational chemistry internship, into coronary artery segmentation for Honours, and then toward astronomy imaging.

But the connecting thread makes sense to me: AI, computer vision, scientific computing, and research. I like using computation to extract information from things that are difficult for humans to analyse directly.

## I am glad I built the Computer Science foundations first

Looking back, I am also glad I spent the last few years building the Computer Science foundations underneath all of this: programming, algorithms, data structures, machine learning, computer vision, high-performance computing, Linux, software engineering, and mathematics.

These subjects often felt separate while I was studying them, but now they are starting to connect.

AI makes it much easier to enter unfamiliar areas quickly, but the foundations still matter. If a model gives me an algorithm, I need enough knowledge to question its complexity. If it gives me a proof, I need enough mathematics to notice an unjustified step. If it generates code, I need enough software knowledge to judge the architecture. If it analyses scientific data, I need enough domain understanding to decide whether the result is meaningful.

The stronger the AI becomes, the more important human judgement seems to become.

## I would love to use AI to find a real breakthrough someday

I do not expect to solve P vs NP next weekend.

But I genuinely want to keep experimenting with AI-assisted research as side projects.

I am much more interested in problems with a real objective, dataset, theorem, experiment, or measurable unknown than in building another thin wrapper around an API.

Maybe the problem will come from computer vision, scientific computing, optimisation, mathematics, or something I have not encountered yet.

The recent mathematical results make this feel much less unrealistic than it would have a few years ago. A student with a computer, strong reasoning models, open research papers, open-source tools, datasets, and formal systems now has an extraordinary amount of intellectual leverage.

Most experiments will go nowhere. That is fine. One genuinely interesting result would already be worth it.

## What happens when AI can do the physical work too?

There is still a much bigger question behind all of this.

If the next ten or fifteen years bring capable robots together with increasingly general AI systems, automation may no longer be limited to information work. Software, writing, analysis, and perhaps large amounts of physical work could all become increasingly automated.

At some point, that raises a strange question:

> If machines can do almost everything useful, what are humans actually supposed to do?

I do not have a good answer. Maybe meaning comes from family, relationships, art, sport, exploration, or simply enjoying life.

For me, research is one of the areas that still feels deeply motivating. Even if AI becomes dramatically more capable, I would still want to participate in the endless pursuit of unanswered questions, unexplained observations, and novel connections between fields.

The role of a researcher may change from doing every calculation manually to deciding which questions are worth asking, designing experiments, connecting domains, testing claims, and directing increasingly capable computational collaborators.

Maybe AI eventually becomes better at those things too.

For now, I am just enjoying watching the frontier move.

## Anyway, back to my own research

The Erdős result surprised me. The prime-gap progress surprised me again, and then ImageCAS-X appeared almost exactly in the research area I am working on.

All of it gave me the same feeling: **research is moving incredibly fast**.

Sometimes that is intimidating. It feels like an invisible race where new competitors and new tools appear without warning.

But it is also exciting.

I have spent the last few years studying Computer Science because I wanted to work with AI and contribute to something meaningful with it. Now I finally feel like I am getting closer to the point where I can actually try.

For now, my goal is much smaller than solving an 80-year-old mathematical problem: finish my Honours year well, do good research, learn as much as I can, make the most of the CSIRO studentship this summer, and keep building enough mathematical, computational, and scientific understanding that one day I might be able to use these tools to find something genuinely new.

That would be pretty cool.
