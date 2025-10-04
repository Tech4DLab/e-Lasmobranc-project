<h1 align="center">🦈 Zero-Shot Elasmobranch Classification Informed by Domain Prior Knowledge 📚</h1>
<h3 align="center">Tech4DLab e_Lasmobranc</h3>

## 🎯 Abstract

The development of systems for the identification of elasmobranchs, including sharks and rays, is crucial for biodiversity conservation and fisheries management, as they represent one of the most threatened marine taxa. This challenge is constrained by data scarcity and the high morphological similarity among species, which limits the applicability of traditional supervised models trained on specific datasets. In this work, we propose an informed zero-shot learning approach that integrates external expert knowledge into the inference process, leveraging the multimodal CLIP framework. The methodology incorporates three main sources of knowledge: detailed text descriptions provided by specialists, schematic illustrations highlighting distinctive morphological traits, and the taxonomic hierarchy that organizes species at different levels. Based on these resources, we design a pipeline for prompt extraction and validation, taxonomy-aware classification strategies, and enriched embeddings through a prototype-guided attention mechanism. The results show significant improvements in CLIP’s discriminative capacity in a complex problem characterized by high inter-class similarity and the absence of annotated examples, demonstrating the value of integrating domain knowledge into methodology development and providing a framework adaptable to other problems with similar constraints.

## 🧮 Architecture

The proposed approach integrates **multiple sources of expert knowledge** to enhance zero-shot classification with CLIP in scenarios with limited data, such as the identification of elasmobranch species. It leverages three main knowledge sources:  
- **Expert descriptions** providing precise textual definitions for each category.  
- **Schematic illustrations** from field guides highlighting distinctive traits.  
- **Hierarchical taxonomy** that organizes categories and groups shared visual features.

The method consists of **two main stages**:

### 🔸 Prompt Extraction and Validation  
<p align="center">
  <img src="images/met1.png" alt="Prompt Extraction and Validation Method" width="900"/>
</p>
Expert-based descriptions and their automatically generated variations are evaluated for their **similarity to visual prototypes** derived from schematic illustrations. The most discriminative prompts are selected by ensuring **intra-class validity** (faithful representation of the target category) and **inter-class validity** (sufficient separation from other classes), which is crucial in low-variability scenarios. Additionally, **taxonomy-aware classification strategies** are applied to exploit the biological hierarchy and reduce ambiguity.

### 🔸 Prototype-Guided Attention  
<p align="center">
  <img src="images/Abstract.png" alt="Prototype-Guided Attention Method" width="900"/>
</p>
Illustrations are used to emphasize distinctive and shared traits within each category when processing real images. These highlighted features are **automatically integrated into CLIP’s attention mechanism**, guiding the model to focus on the most relevant regions and improving alignment with the optimized prompts.

## 📈 Performance

The table below summarizes the performance of CLIP when using different **name descriptors** and **taxonomic levels** for zero-shot classification. Results are reported at the **order**, **family**, and **species** levels, the latter using both **scientific (SC)** and **common names (CN)**. Metrics include **balanced accuracy (BAcc)**, **accuracy (Acc)**, and **weighted precision, recall, and F1-score**.

| Level          | BAcc | Acc | Prec | Rec | F1  |
|---------------|:----:|:---:|:----:|:---:|:---:|
| Order         | 0.37 | 0.70 | 0.70 | 0.70 | 0.36 |
| Family        | 0.18 | 0.18 | 0.20 | 0.18 | 0.16 |
| Species (SC)  | 0.13 | 0.15 | 0.21 | 0.15 | 0.14 |
| Species (CN)  | 0.37 | 0.51 | 0.36 | 0.51 | 0.40 |

The results show that the model had not encountered this type of data during its pretraining.

**🔥 Our work:** 
The results indicate that incorporating taxonomy-aware strategies and attention guidance substantially improves performance across all taxonomic levels compared to the baseline prompt-based approach. Sequential taxonomy-aware classification combined with selective attention bias (P-TS+SB) achieves the best overall results, particularly at the species level.

| Level   | Method    | BAcc | Acc | Prec | Rec | F1  |
|---------|-----------|:----:|:---:|:----:|:---:|:---:|
| **Order**   | PA        | 0.79 | 0.79 | 0.86 | 0.79 | 0.81 |
|         | P-TC      | 0.83 | 0.82 | 0.89 | 0.82 | 0.84 |
|         | P-TS      | 0.83 | 0.82 | 0.89 | 0.82 | 0.84 |
|         | P-TS+B    | 0.82 | 0.82 | 0.88 | 0.82 | 0.84 |
|         | P-TS+SB   | 0.83 | 0.82 | 0.89 | 0.82 | 0.84 |
| **Family**  | PA        | 0.70 | 0.74 | 0.76 | 0.74 | 0.74 |
|         | P-TC      | 0.78 | 0.79 | 0.80 | 0.79 | 0.79 |
|         | P-TS      | 0.78 | 0.75 | 0.81 | 0.75 | 0.77 |
|         | P-TS+B    | 0.78 | 0.77 | 0.81 | 0.77 | 0.78 |
|         | P-TS+SB   | 0.79 | 0.77 | 0.82 | 0.77 | 0.78 |
| **Species** | PA        | 0.51 | 0.50 | 0.67 | 0.50 | 0.48 |
|         | P-TC      | 0.61 | 0.58 | 0.69 | 0.58 | 0.57 |
|         | P-TS      | 0.62 | 0.60 | 0.70 | 0.60 | 0.61 |
|         | P-TS+B    | 0.64 | 0.63 | 0.70 | 0.63 | 0.64 |
|         | P-TS+SB   | 0.64 | 0.63 | 0.71 | 0.63 | 0.64 |


The confusion matrices below show the performance of the **P-TS** (taxonomy-aware sequential classification) and **P-TS+B** (with CLIP attention bias) methods across different taxonomic levels, with a focus on family and species.  
Classes are **ordered by visual similarity** and follow the **order–family–species structure**, which causes errors to cluster in **neighboring cells**.  

P-TS serves as the baseline, showing higher or similar error rates in fine-grained categories, while **P-TS+B achieves the most significant improvements**, especially in the most challenging groups. A slight performance drop is observed in one typically easy class, likely due to the integration of fine-grained attention maps. The **P-TS+SB variant**, applied selectively at family and species levels, further refines distinctions in the hardest categories, though it remains slightly below P-TS+B overall.

<p align="center">
  <img src="images/confus.png" alt="Prototype-Guided Attention Method" width="900"/>
</p>

## 🧠 Attention Maps 

The following examples illustrate **attention maps generated by the proposed prototype-guided attention mechanism**. These visualizations highlight the image regions that contribute most to the model’s classification decisions when matching real images with optimized textual prompts.

Although the attention maps generally emphasize distinctive morphological traits, **some noise remains**, especially in background regions or secondary elements. This is expected because CLIP was pretrained on large-scale, diverse datasets without domain-specific supervision. As a result, the model occasionally activates irrelevant areas, particularly in low-contrast or cluttered images.  
Despite this, the integration of schematic prototypes significantly improves focus on taxonomically relevant regions compared to vanilla CLIP.

<p align="center">
  <img src="images/heat.png" alt="Prototype-Guided Attention Method" width="900"/>
</p>

## ⚙️ Effect of Attention Bias Configuration

The figure below shows the impact of different **α values** (bias multipliers) and **modified layers** within the CLIP architecture on performance.  
Both **balanced accuracy** (left) and **accuracy** (right) are reported. Adjusting α and selecting appropriate layers significantly influences the model’s ability to focus on relevant regions, with certain configurations yielding clear performance gains.

<p align="center">
  <img src="images/bias_config.png" width="600"/>
</p>

# 🏁 Conclusions

This work demonstrates the feasibility of **integrating prior and domain-specific knowledge** into **informed zero-shot classification** to address challenges such as extreme data scarcity. By leveraging expert resources — including **textual descriptions**, **schematic illustrations**, and **taxonomic structures** — CLIP’s potential is adapted to a domain with limited visual data but abundant structured knowledge.

The proposed framework is **reproducible**, **adaptable**, and applicable to other domains facing similar constraints of scarce data and high inter-class similarity.  
Additionally, it tackles a **largely unexplored task**: the **identification of elasmobranch species in out-of-water contexts**, which is crucial for **fisheries management**, **bycatch control**, and the **conservation of endangered species**.

# 🤝 Aknoledgments

eLasmobranc project is developed with the collaboration of the Biodiversity Foundation of the Ministry for Ecological Transition and the Demographic Challenge, through the Pleamar Programme, and is co-financed by the European Union through the European Maritime, Fisheries and Aquaculture Fund.
