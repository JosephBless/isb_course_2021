<!-- .slide: data-background="assets/isb/data-midnight.jpg" class="dark" -->

# Building metabolic models of bacterial isolates

### Christian Diener, Gibbons Lab

<img src="assets/isb/logo.png" width="40%">

from the *2021 ISB Virtual Microbiome Series*

<br>
<div class="footer">
<a href="https://creativecommons.org/licenses/by-sa/4.0/"><i class="fa fa-bullhorn"></i>CC-BY-SA</a>
<a href="https://gibbons.isbscience.org/"><i class="fa fa-globe"></i>gibbons.isbscience.org</a>
<a href="https://github.com/gibbons-lab"><i class="fa fa-github"></i>gibbons-lab</a>
<a href="https://twitter.com/thaasophobia"><i class="fa fa-twitter"></i>@thaasophobia </a>
</div>

---

<!-- .slide: data-background="var(--primary)" class="dark" -->

Let's get the slides first (use your computer, phone, TV, fridge)

*https://gibbons-lab.github.io/isb_course_2021/models*

---

<!-- .slide: data-background="var(--primary)" class="dark" -->

## Quick reminder :clock:

<img src="assets/materials.png" width="80%">

<br>

<a href="https://colab.research.google.com/github/gibbons-lab/isb_course_2021/blob/main/models.ipynb"
   target="_blank">Click me to open the notebook!</a>

---

# Functional analyses

Tries to predict what the microbiome *does* from sequencing data.

Uses gene/transcript/protein/metabolite abundances (metagenomics, metatranscriptomics, proteomics or metabolomics).

Gene content provides insights into metabolic *capacity* or *potential*.

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

# Genes and metabolite abundances are cool and all, but what you should really care about are metabolic fluxes*

<div class="footnote">

hot take :fire:

</div>


---

## Fluxes

<img src="assets/fluxes.png" width="45%">
<video width="45%" autoplay loop>
  <source src="assets/fluxes.mp4" type="video/mp4">
</video>

<div class="footnote">

video courtesy of [S. Nayyak](https://twitter.com/Na_y_ak) and [J. Iwasa](https://twitter.com/janetiwasa)

</div>

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

# Flux Balance Analysis (FBA)

Can we infer the most likely fluxes in a biological system, given a set of environmental constraints, if we know all
available metabolic reactions?

---

## The flux cone

<img src="assets/flux_cone.png" width="100%">


---

The goal of FBA is to *reduce* the overall flux space to a *biologically relevant* one.

---

## Genome-scale metabolic modeling

<img src="assets/fba.png" width="100%">

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

# How do we get from sequencing data to metabolic reactions?

Metabolic reactions are catalyzed by an organism's *enzymes*, which are encoded
in its *genes*.

So what we need is a *genome*.

---

## Genome Assembly with De Bruijn graphs

![](assets/de_bruijn.png)

---

## Not so straight-forward

<img src="assets/assembly_graph_k59.png" height="700vh">

---

## Finding genes

<img src="assets/prodigal.png" width="80%">

- finds open reading frames (ORFs)
- identifies ribosomal binding sites and spacers
- *really good* at identifying proteins

<span class="footnote">

https://github.com/hyattpd/Prodigal

</span>

---

<img src="assets/right.jpg" width="50%">

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

# Genome-Scale Metabolic Model Reconstruction

When mapping to reference databases, we will only ever identify a small set (~1/3) of all enzymes and transporters in a given genome.

We need to fill in the critical gaps in metabolic pathways by imposing *functional requirements* on the model.

What's a reasonable requirement that you'd attribute to a living bacterium?

---

## General strategy

<img src="assets/reconstruction.png" width="80%">

---

<img src="assets/reconstruction_strategies.png" width="90%">

---

## Curated reconstructions

*Curation* is the process of adding or removing reactions to/from the model based on experimental evidence.

<br>

#### Basic

- check structural quality ([MEMOTE](https://doi.org/10.1038/s41587-020-0446-y))
- gap-filling on a standard growth medium (LB, M9, ...)
- example: [carveME EMBL GEMs](https://github.com/cdanielmachado/embl_gems) - 5587 strains

<br><br>

#### Stringent

- growth on various carbon sources (e.g. "likes maltose, but not glucose")
- known metabolic conversion (e.g. "produces indole from tryptophan")
- strain-specific biomass composition
- example: [AGORA](https://www.vmh.life/#microbes/) - 818 strains from human gut


---

|                | [carveME](https://doi.org/10.1093/nar/gky537) | [ModelSEED/Kbase](https://doi.org/10.1038/nbt.1672) | [gapseq](https://doi.org/10.1186/s13059-021-02295-1) |
|----------------|-----------|-----------------|------------|
| speed          | :smile:   | :pensive:       | :cry:      |
| sensitivity    | :pensive: | :cry:           | :smile:    |
| model quality  | :smile:   | :pensive:       | :smile:    |
| free solver    | :cry:     | :pensive:       | :smile:    |
| easy install   | :smile:   | :smile:         | :cry:      |
| easy to use    | :pensive: | :smile:         | :cry:      |
| many media     | :pensive: | :smile:         | :smile:*   |
| SBML standard  | :smile:   | :smile:         | :smile:    |


## Limitations

- unknown enzymes/pathways are never captured
- dependency on a universal model
- hard to formulate growth media
- growth objective may not always apply (e.g. toxicity, human tissues)

---

<!-- .slide: data-background="assets/isb/microbes-midnight.jpg" class="dark" -->

# The BIO-ML collection

Sequenced 3,632 bacterial isolates from the human gut at the Broad Institute.

Each participant will get a randomly assigned assembly from ~980 of those.

<div class="footnote">

https://doi.org/10.1038/s41591-019-0559-3

</div>

---

## Your turn

<img src="assets/coding.gif" width="50%">

Let's go from an isolate genome assembly to a genome-scale metabolic model.

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

# Optimal growth rate ≠ unique fluxes

Even though FBA yields a unique estimate of the maximum growth rate, it does not give us a unique flux solution.

Can we constrain the flux space even further?

---

## Selecting biologically relevant fluxes via parsimony

<img src="assets/pfba.png" width="70%">

- Bacteria do not like to produce more enzymes than necessary.
- Zero flux = no enzyme required.
- Parsimony reproduces experimental fluxes in <i>E. coli</i> [very well](https://dx.doi.org/10.1038%2Fmsb.2010.47).

---

<!-- .slide: data-background="var(--secondary)" class="dark" -->

FBA results strongly depend on the *environmental conditions*. Finding
uptake constraints is *critical* for realistic results.

Quantitative > qualitative >> unconstrained

Uptake and secretion fluxes are much easier to measure than internal fluxes (e.g. you can use longitudinal metabolomics).

---

## Your turn

Let's estimate the fluxes for our models.

<img src="assets/coding.gif" width="50%">

---

<!-- .slide: data-background="assets/isb/microbes-azure.jpg" class="dark" -->

# Break :clock:

---

<!-- .slide: data-background="assets/isb/data-midnight.jpg" class="dark" -->

# Reverse ecology: inferring environmental interactions with metabolic models

### Christian Diener, Gibbons Lab

<img src="assets/isb/logo.png" width="40%">

from the *2021 ISB Virtual Microbiome Series*

<br>
<div class="footer">
<a href="https://creativecommons.org/licenses/by-sa/4.0/"><i class="fa fa-bullhorn"></i>CC-BY-SA</a>
<a href="https://gibbons.isbscience.org/"><i class="fa fa-globe"></i>gibbons.isbscience.org</a>
<a href="https://github.com/gibbons-lab"><i class="fa fa-github"></i>gibbons-lab</a>
<a href="https://twitter.com/thaasophobia"><i class="fa fa-twitter"></i>@thaasophobia </a>
</div>


---

<!-- .slide: data-background="var(--primary)" class="dark" -->

# Reverse ecology

[Ecology](https://en.wikipedia.org/wiki/Ecology) is the study of the relationships between living organisms and their physical environment.

[Reverse Ecology](https://en.wikipedia.org/wiki/Reevrse_ecology) is the study of an organism's genome as a means to estimate its ecological niche.

---

## Wait, why did we just stumble into Reverse Ecology?

Systems Biology lingo:
>All strains were simulated under the exact same environmental conditions, but they each showed different *uptake rates* for different sets of metabolites to obtain their *maximum growth rate*.

Ecology lingo:
>We estimated the *realized niche* of bacterial strains grown in isolation under an *optimal fitness* objective.

What would it mean ecologically for two strains to inhabit very similar metabolic niches, or very different ones? Think about the human gut, where many of these strains may be present at the same time.

---

## Your turn

Let's combine *all* the results.

<img src="assets/coding.gif" width="50%">

---

<!-- .slide: data-background="assets/isb/microbes-azure.jpg" class="dark" -->

### And we are done :clap:

<div style="display: flex; justify-content: space-around; align-items: center;">

*ISB team* <br>
Joey Petosa <br>
Allison Kudla <br>
Christian Diener <br>
Sean Gibbons <br>
Priyanka Baloni <br>
Tomasz Wilmanski <br>
Noa Rappaport <br>
Alex Carr <br>
Audri Hubbard <br>
Renee Duprel <br>
Joe Myxter <br>
Thea Swanson

# Thanks!

</div>
