---
layout: page
title: Projects
permalink: /projects/
---

Four computational projects, each built to take one specific claim from the
[literature reviews](/publications/) and turn it into something I could measure myself.

None of them is laboratory work and none produced a new scientific finding. The first
two are simplified, illustrative models; the last two are measurements of a known
problem, made on other people's data and software. Each has an English write-up and an
interactive page that runs in the browser with no installation.

## Why glucose is burned to lactate

*Flux balance analysis · [repository](https://github.com/officeofalper/cancer-flux-balance) · [interactive page](https://officeofalper.github.io/cancer-flux-balance/web/)*

A 40-reaction metabolic network of a single cancer cell, solved as a linear program.
The question: why would a cell throw away most of the energy in glucose when its
mitochondria work perfectly well?

Adding one constraint — that enzymes and mitochondria compete for limited room inside
the cell — is enough. Above a threshold of roughly 16, lactate production appears on
its own, without any defect. The model also shows why single-target drugs disappoint:
blocking glutaminase cuts growth only to 89%, because pyruvate carboxylase takes over
the same job.

## Tumour cells and T cells competing for food

*Differential equations and an agent-based spatial model · [repository](https://github.com/officeofalper/tme-glucose-competition) · [interactive page](https://officeofalper.github.io/tme-glucose-competition/web/)*

Can a tumour disable the immune cells sent to kill it simply by eating first? In the
model there is a sharp threshold: a tumour starting at size 21 is cleared, one starting
at 22 escapes. Past that point a feedback loop closes — less glucose, weaker T cells,
less killing, a bigger tumour.

A drug that blocks glucose uptake reverses this, but only while it stays selective. T
cells run on the same pathway, so once the side effect on them passes about 70%, the
treatment fails. The whole result rests on one assumption, which I state plainly in the
write-up: that tumour cells take up glucose more avidly than T cells do.

## Did the model learn binding, or memorise the benchmark?

*Machine learning on LP-PDBBind · [repository](https://github.com/officeofalper/binding-affinity-leakage) · [interactive page](https://officeofalper.github.io/binding-affinity-leakage/web/)*

Affinity prediction models report high accuracy. How much of it survives a test set
that was built honestly? The same random forest, on the same data, drops from r = 0.67
to 0.54 when only the train/test split changes.

The sharpest result is a model shown **only the protein** and never the drug: it cannot
represent binding at all, yet under a random split it scores 0.64. The only thing it
can be doing is recognising the dataset. Leakage does not explain everything, though,
and I say where it stops explaining things.

## When the docking input cannot support a correct answer

*Molecular docking into twelve crystal structures · [repository](https://github.com/officeofalper/sirt-docking) · [interactive page](https://officeofalper.github.io/sirt-docking/web/)*

An inhibitor that opens its own binding pocket in SIRT2, docked into twelve structures of
the same protein. Into the crystal it came from, docking is near-perfect (0.35 Å). Into
the unbound enzyme it is 11 Å wrong and never right — because the correct pose does not
fit there at all: 26 of the ligand's 29 heavy atoms overlap the protein.

So the search did not make a mistake. It was asked to place a molecule in a pocket that
is not open, and it reported the best available alternative with a respectable score and
no warning. Two further structures separate the two possible causes, and the answer is
the pocket rather than the missing ligand. The most troubling case is a structure whose
pocket was opened by a *different* inhibitor: it gives the best score in the whole study
while being 9.7 Å wrong.

## Does a docking rule travel? Testing my own result on a second target

*Eight HSP90 structures, one reference ligand · [repository](https://github.com/officeofalper/hsp90-docking) · [interactive page](https://officeofalper.github.io/hsp90-docking/web/)*

The SIRT2 study above ended with a rule: among a target's co-crystal structures, pick the
one whose bound ligand most resembles your compound. The separation there was perfect. But
I had chosen SIRT2 precisely because its induced fit is extreme, so the rule might only
describe SIRT2. This is the attempt to break it.

Two things went wrong before any result existed, and both are in the write-up. The protocol
did not transfer: deleting waters is right on SIRT2 and destroys binding on HSP90, where
the inhibitor reaches Asp93 through a bridging water. The self-docking control caught that
at 6.29 Å. Then, after the results were written, I found that my clash count read only
protein atoms while the docking ran against protein plus the retained waters — which had
inverted a correlation from +0.80 to −0.08 and produced the study's most striking claim as
an artefact.

Corrected: the trend transfers and the steric mechanism with it, but the clean separation
does not. A structure at Tanimoto 0.359 fails where one at 0.333 succeeds, because on HSP90
the feature the ligand shapes is an ordered water network, and a whole-molecule fingerprint
cannot see which part of a molecule rearranges it. The rule is narrower than I wrote it.
