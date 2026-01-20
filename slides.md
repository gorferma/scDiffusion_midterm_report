---
theme: seriph
background: Images/Title_page.jpg
class: text-center
highlighter: shiki
lineNumbers: false
routerMode: hash
info: |
  ## scDiffusion Presentation
  Presentation for scDiffusion project.
drawings:
  persist: false
transition: slide-left
title: scDiffusion
---

# scDiffusion
### Biological Vision, Data Reality & Model Runs

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Start <carbon:arrow-right class="inline"/>
  </span>
</div>

---

# Agenda

1. **Intro & Biological Vision**
2. **The Data Reality ("The Swamp")**
3. **The Model Runs (The Learning Curve)**
4. **Outlook & Discussion**

---
layout: section
---

# I. Intro & Biological Vision

---

## What is scDiffusion?

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

<div class="space-y-2">

<div v-click class="bg-slate-800 p-3 rounded-lg border-l-4 border-teal-500 shadow-xl transition-all duration-500">
  <div class="flex items-center gap-2 mb-1">
    <carbon:chemistry class="text-teal-400 text-lg"/>
    <h3 class="font-bold text-base m-0 text-white">Definition</h3>
  </div>
  <p class="text-xs text-slate-100 m-0 leading-tight">
    A probabilistic generative model that learns to reverse the diffusion process to generate high-fidelity single-cell data from noise.
  </p>
</div>

<div v-click class="bg-slate-800 p-3 rounded-lg border-l-4 border-purple-500 shadow-xl transition-all duration-500">
  <div class="flex items-center gap-2 mb-1">
    <carbon:chart-line-data class="text-purple-400 text-lg"/>
    <h3 class="font-bold text-base m-0 text-white">Core Concept</h3>
  </div>
  <p class="text-xs text-slate-100 m-0 leading-tight">
    It iteratively refines random noise into biologically meaningful gene expression profiles.
  </p>
</div>

<div v-click class="bg-slate-800 p-3 rounded-lg border-l-4 border-yellow-500 shadow-xl transition-all duration-500">
  <div class="flex items-center gap-2 mb-1">
    <carbon:idea class="text-yellow-400 text-lg"/>
    <h3 class="font-bold text-base m-0 text-white">Analogy</h3>
  </div>
  <p class="text-sm text-slate-100 m-0 leading-tight">
    Think of it as "DALL-E for Cells" — generating realistic biological data from scratch.
  </p>
</div>

</div>

</div>

<div class="flex items-center justify-center">
  <DogDenoise />
</div>

</div>

---

<h2 class="text-center">scDiffusion - Capabilities</h2>

<div class="grid grid-cols-2 gap-x-12 gap-y-8 mt-12 px-10">

<div v-click class="flex items-center gap-4 bg-white/90 p-6 rounded-xl shadow-lg border-b-4 border-teal-500 transition-all duration-500">
  <carbon:data-blob class="text-teal-600 text-4xl flex-shrink-0" />
  <div>
    <h3 class="font-bold text-black text-xl m-0">Data Augmentation</h3>
    <p class="text-base text-gray-800 m-0 leading-tight">Generating synthetic cells to balance rare cell types.</p>
    <div class="text-xs text-teal-800 font-semibold mt-1 uppercase tracking-wider">Unconditional & Conditional</div>
  </div>
</div>

<div v-click class="flex items-center gap-4 bg-white/90 p-6 rounded-xl shadow-lg border-b-4 border-purple-500 transition-all duration-500">
  <carbon:pills class="text-purple-600 text-4xl flex-shrink-0" />
  <div>
    <h3 class="font-bold text-black text-xl m-0">In-silico Perturbation</h3>
    <p class="text-base text-gray-800 m-0 leading-tight">Generating cell populations for specific conditions (e.g. Cancer vs. Healthy).</p>
    <div class="text-xs text-purple-800 font-semibold mt-1 uppercase tracking-wider">Multiconditional generation</div>
  </div>
</div>

<div v-click class="col-span-2 justify-self-center w-[48%] flex items-center gap-4 bg-white/90 p-6 rounded-xl shadow-lg border-b-4 border-yellow-500 transition-all duration-500">
  <carbon:chart-relationship class="text-yellow-600 text-4xl flex-shrink-0" />
  <div>
    <h3 class="font-bold text-black text-xl m-0">Trajectory Interpolation</h3>
    <p class="text-base text-gray-800 m-0 leading-tight">Mapping developmental transitions.</p>
    <div class="text-xs text-yellow-800 font-semibold mt-1 uppercase tracking-wider">Gradient interpolation</div>
  </div>
</div>

</div>

---

<h2 class="text-center">scDiffusion - Architecture</h2>

<div class="flex justify-center items-center h-full">
  <img src="./Images/model_archi.png" class="max-h-110 rounded-lg shadow-xl" />
</div>

---
layout: section
---

# II. The Data Reality: From Search to Swamp

---

## Part 1: The Search for the "Perfect" Dataset

<div class="grid grid-cols-2 gap-8 mt-6">

<div>
  <div v-click="1" class="bg-red-50 p-4 rounded-lg border-l-4 border-red-500 mb-6 opacity-80">
    <h3 class="font-bold text-red-800 m-0 text-sm">Initial Dataset: Bladder Cancer Oh 2020</h3>
    <p class="text-xs m-0 text-red-900">Problem: Already normalized. Unsuitable for my teammates.</p>
    <p class="text-xs text-red-600 mt-1">→ Need raw signal for training.</p>
  </div>

  <div v-click="2">
    <div class="font-bold text-xl mb-2">My Search Criteria:</div>
    <ul class="space-y-3">
      <li class="flex items-center gap-2">
        <carbon:checkmark-filled class="text-green-500 text-xl flex-shrink-0"/> 
        <span>Raw UMIs: Essential for compatibility with all methods.</span>
      </li>
      <li class="flex items-center gap-2">
        <carbon:checkmark-filled class="text-green-500 text-xl flex-shrink-0"/> 
        <span>Volume: Sufficient cells for stable training.</span>
      </li>
      <li class="flex items-center gap-2">
        <carbon:checkmark-filled class="text-green-500 text-xl flex-shrink-0"/> 
        <span>Complexity: Rare cell types & intermediate states (to showcase interpolation).</span>
      </li>
    </ul>
  </div>
</div>

<div v-click="3" class="flex flex-col justify-center items-center bg-teal-50 rounded-xl border-2 border-teal-500 p-6 shadow-lg">
  <div class="text-teal-800 font-bold text-2xl mb-2 text-center">The Candidate: AML Van Galen 2019</div>
  <carbon:certificate-check class="text-6xl text-teal-500 mb-4"/>
  <p class="text-center text-sm">
    Seemed perfect on paper.<br>
    QC was reportedly done.
  </p>
</div>

</div>

---

## Part 2: The Metadata Swamp

<div class="grid grid-cols-2 gap-10 mt-6">

<div>

<h3 v-click="1" class="text-xl font-bold text-purple-700 mb-4">The Reality Check</h3>

<div v-click="1" class="relative bg-white border border-gray-300 shadow-md p-1 transform rotate-1 mb-4">
  <div class="text-[10px] font-mono text-gray-400 bg-gray-50 p-2 overflow-hidden h-32 transition-all duration-300" :class="$slidev.nav.clicks >= 2 ? 'blur-[1px]' : ''">
    Cell_01: TranscriptomeUMIs=2400, nCount_RNA=2550 (Conflict)<br>
    Cell_02: NumberOfGenes=500, nFeature_RNA=520 (Conflict)<br>
    Artifact_X: Outdated_QC_Metrics...<br>
    ...
  </div>
  
  <div v-click="2" 
       class="absolute top-8 left-8 border-4 border-red-600 text-red-600 font-bold text-xl p-2 rounded rotate-[-15deg] bg-white/80 transition-all duration-200 ease-in transform"
       :class="$slidev.nav.clicks >= 2 ? 'scale-100 opacity-100' : 'scale-300 opacity-0'">
    CONTRADICTORY LABELS
  </div>
</div>

<div v-click="2" class="text-sm mt-4">

**Discovery:** Metadata contained conflicting technical metrics (e.g. legacy vs. updated UMI counts).

</div>

</div>

<div>

<h3 v-click="3" class="text-xl font-bold text-teal-700 mb-4">The Solution: Re-Validation</h3>

<div class="space-y-4 text-sm">

<div v-click="4" class="bg-white p-3 rounded shadow-sm border-l-4 border-teal-500">

**Algorithmic Check:**
<div class="text-xs text-gray-600 mt-1">Validated technical labels (nCounts, nFeature_RNA) directly from raw UMI data.</div>

</div>

<div v-click="5" class="bg-white p-3 rounded shadow-sm border-l-4 border-blue-500">

**Metadata Categorization:**
<div class="text-xs text-gray-600 mt-1">Systematically mapped metadata fields into functional categories.</div>

<div class="mt-2 flex gap-4 text-[10px] font-mono bg-gray-100 p-2 rounded justify-center">
  <span class="text-blue-600">● Model Inputs</span>
  <span class="text-teal-600">● Validation</span>
  <span class="text-red-500">● Artifacts</span>
</div>

</div>

</div>

</div>

</div>

---
layout: section
---

# III. The Model Runs (The Learning Curve)

---

## My Workflow: Controlled Iteration

<div class="grid grid-cols-2 gap-8 mt-6">

<div v-click="1" class="space-y-4">
<h3 class="font-bold text-teal-700">1. Notebook-Driven Process</h3>
<div class="bg-slate-100 p-3 rounded-lg border-l-4 border-teal-500 text-sm">
<p class="mb-2">I use Notebooks for three purposes:</p>
<ul class="list-disc pl-4 space-y-1">
<li><strong>Training:</strong> Model configuration & training loop.</li>
<li><strong>Generation:</strong> Sampling from the trained model.</li>
<li><strong>Evaluation:</strong> Distinct notebooks for each generation type to ensure rigorous assessment.</li>
</ul>
</div>
</div>

<div v-click="2" class="space-y-4">
<h3 class="font-bold text-purple-700">2. Run Isolation & Logging</h3>
<div class="bg-slate-100 p-3 rounded-lg border-l-4 border-purple-500 text-sm">
<p class="font-bold mb-1">Codebase Snapshots:</p>
<p class="mb-3">Each run gets a dedicated <code>scDiffusion</code> copy to allow specific tweaks without breaking others.</p>
<p class="font-bold mb-1">Documentation:</p>
<p class="mb-1">A dedicated <code>README</code> logs every change:</p>
<ul class="list-disc pl-4 space-y-1">
<li>Toggling <code>scimilarity</code> integration.</li>
<li>Normalization logic (Built-in vs. Manual) for Whole Data vs. HVGs.</li>
<li>Fix hardcoded values</li>
</ul>
</div>
</div>

</div>

---

## Run 1: The Pipeline Error (Part I)

<div class="grid grid-cols-2 gap-8 mt-6">

<div class="space-y-4">
<div v-click="1">
<h3 class="font-bold text-teal-700">The Setup</h3>
<div class="bg-slate-100 p-3 rounded-lg border-l-4 border-teal-500 text-sm">
<p class="mb-2">Goal: Fast iteration & pipeline testing.</p>
<ul class="list-disc pl-4 space-y-1">
<li>Data: Subset to 2000 HVGs (Highly Variable Genes).</li>
<li>Expectation: Pipeline handles raw counts + <code>celltype</code> column.</li>
</ul>
</div>
</div>

<div v-click="2">
<h3 class="font-bold text-red-700">The Symptoms</h3>
<div class="bg-red-50 p-3 rounded-lg border-l-4 border-red-500 text-sm">
<ul class="list-disc pl-4 space-y-1">
<li>AE Loss: Steady (looked normal).</li>
<li>Latent Space: Collapsed into a "Big Blob".</li>
<li>Generated UMAPs: (See next slide).</li>
</ul>
</div>
</div>
</div>

<div class="flex flex-col gap-4 items-center justify-start -mt-16">
<div v-click="2" class="w-full h-56 flex items-center justify-center">
  <img src="./Images/VAE_Training_Loss_From_Scratch_HVG2000.png" class="h-full object-contain rounded-lg shadow-sm" alt="VAE Loss Graph" />
</div>
<div v-click="2" class="w-full h-56 flex items-center justify-center">
  <img src="./Images/VAE_UMAP_Latent_Space_HVG2000_From_Scratch.png" class="h-full object-contain rounded-lg shadow-sm" alt="Latent Space UMAP" />
</div>
</div>

</div>

---

## Run 1: The Pipeline Error (Part II)

<div class="grid grid-cols-2 gap-8 mt-2">

<div v-click="1" class="flex flex-col items-center">
  <h3 class="font-bold text-teal-700 mb-2">Unconditional Generation</h3>
  <img src="./resources/wrong_library_size_run/Unconditional_UMAP_Unscaled.png" class="h-60 object-contain rounded-lg shadow-sm border border-gray-200" />
  <div class="mt-4 bg-yellow-50 p-3 rounded-lg border-l-4 border-yellow-500 text-sm text-yellow-800 w-full">
    <p class="font-bold">The Paradox:</p>
    <p>At first glance, the manifold is learned correctly!</p>
    <p class="text-xs text-gray-600 mt-1">Confusing, given the collapsed Latent Space.</p>
  </div>
</div>

<div v-click="2" class="flex flex-col items-center">
  <h3 class="font-bold text-red-700 mb-2">Conditional Generation (B-Cells)</h3>
  <img src="./resources/wrong_library_size_run/unscaled/conditional_umap_B.png" class="h-60 object-contain rounded-lg shadow-sm border border-gray-200" />
  <div class="mt-4 bg-red-50 p-3 rounded-lg border-l-4 border-red-500 text-sm text-red-800 w-full">
    <p class="font-bold">The Reality Check:</p>
    <p>Specific cell type generation fails completely.</p>
    <p class="text-xs text-gray-600 mt-1">The classifier cannot navigate the unstructured "Blob" latent space.</p>
  </div>
</div>

</div>

---

## Run 1: The Post-Mortem

<div class="grid grid-cols-2 gap-8 mt-4">

<div v-click="1" class="space-y-4">
  <div class="bg-red-50 p-3 rounded-lg border-l-4 border-red-500 text-sm">
    <h3 class="font-bold text-red-800 m-0 text-base">The Root Cause: Normalization</h3>
    <ul class="list-disc pl-4 mt-2 space-y-1 text-red-900">
      <li>The Dataloader normalized based on input size.</li>
      <li>Feeding HVGs only meant calculating library size on a fraction of the data.</li>
      <li>Result: Counts no longer represented the true cellular state.</li>
    </ul>
  </div>

  <div class="bg-blue-50 p-3 rounded-lg border-l-4 border-blue-500 text-sm">
    <h3 class="font-bold text-blue-800 m-0 text-base">Why Unconditional looked "Good"</h3>
    <p class="text-blue-900 mt-1">"Mud In = Mud Out"</p>
    <p class="text-xs text-blue-800 mt-1">
      I applied the <em>same faulty normalization</em> to the real data for comparison. The Diffusion model is so powerful it perfectly replicated this meaningless "mush".
    </p>
  </div>
</div>

<div v-click="2" class="space-y-4">
  <div class="bg-gray-100 p-3 rounded-lg border-l-4 border-gray-500 text-sm">
    <h3 class="font-bold text-gray-800 m-0 text-base">Why Conditional Failed</h3>
    <p class="text-gray-900 mt-1">
      The Classifier needs structure to guide generation.
    </p>
    <p class="text-xs text-gray-800 mt-1">
      Since the VAE failed to create a structured latent space (no clusters), the Classifier was steering blindly in a fog.
    </p>
  </div>

  <div class="bg-yellow-50 p-4 rounded-lg border-l-4 border-yellow-500 text-sm shadow-md">
    <h3 class="font-bold text-yellow-800 m-0 text-base flex items-center gap-2">
      <carbon:light-filled /> The Lesson
    </h3>
    <p class="text-yellow-900 mt-2 font-semibold">
      Never rely on Loss Curves alone!
    </p>
    <p class="text-xs text-yellow-800 mt-1">
      Always validate the VAE's Latent Space to ensure it has learned biologically meaningful representations.
    </p>
  </div>
</div>

</div>

---

## Run 2: The Fix

<div class="grid grid-cols-2 gap-8 mt-6">

<div class="space-y-6">
  <div v-click="1">
    <h3 class="font-bold text-teal-700 text-xl">The Correction</h3>
    <div class="bg-slate-100 p-4 rounded-lg border-l-4 border-teal-500">
      <p>Action: Normalized the full dataset <em>before</em> HVG selection.</p>
      <p class="mt-2 text-sm text-gray-600">This preserved the true library size information.</p>
    </div>
  </div>
  
  <div v-click="2">
    <h3 class="font-bold text-purple-700 text-xl">The Result</h3>
    <p class="text-gray-700 mb-2">A beautifully structured Latent Space.</p>
    <div class="flex items-center gap-2 text-sm text-green-600 font-bold bg-green-50 p-2 rounded">
      <carbon:checkmark-filled /> VAE Training Successful
    </div>
  </div>
</div>

<div v-click="2" class="flex flex-col items-center">
  <img src="./resources/proper_normalization_run/Latent_Space_UMAP.png" class="h-60 rounded-lg shadow-lg border-2 border-teal-500" />
  <p class="text-sm text-teal-700 mt-2 font-bold">Clear Separation of Clusters!</p>
</div>

</div>

---

## The Plot Twist

<div class="grid grid-cols-2 gap-8 mt-2">

<div v-click="1">
  <h3 class="font-bold text-gray-700 mb-2">The Expectation</h3>
  <p class="text-sm mb-2">
    With a perfect Latent Space, the generated cells should look perfect, right?
  </p>
  <div class="bg-gray-100 p-4 rounded-lg border-l-4 border-gray-500 italic text-sm">
    "Let's look at the standard Z-Scored UMAP..."
  </div>
</div>

<div v-click="2" class="flex flex-col items-center">
  <h3 class="font-bold text-red-600 mb-2">The Reality (Shock!)</h3>
  <img src="./resources/proper_normalization_run/Unconditional_overlay_scaled.png" class="h-45 object-contain rounded border-2 border-red-500" />
  <p class="text-xs text-red-600 mt-2 font-bold">It looks worse than Run 1?!</p>
</div>

</div>

<div v-click="3" class="mt-2 bg-yellow-50 p-3 rounded-lg border-l-4 border-yellow-500 text-center">
  <h3 class="text-yellow-800 font-bold m-0">The Confession</h3>
  <p class="text-sm text-yellow-900 m-0">
    The "Good" image I showed you in Run 1? That was unscaled.<br>
    This "Garbage" is what the standard pipeline actually outputs.
  </p>
</div>

---

## The Detective Work: VAE Noise

Why does the "Standard" view look so bad when the model is good?

<div class="grid grid-cols-2 gap-6 mt-4">

<div v-click="1" class="flex flex-col items-center">
  <div class="text-xs font-bold uppercase text-green-500 mb-1">Unscaled (Truth)</div>
  <img src="./resources/proper_normalization_run/Unconditional_overlay_unscaled.png" class="h-40 object-contain rounded border-2 border-green-200" />
  <p class="text-[10px] text-center mt-1 w-60">The manifold is perfect.</p>
</div>

<div v-click="2">
  <div class="text-xs font-bold uppercase text-blue-500 mb-1">The Data Proof</div>
  <table class="text-[10px] w-full border-collapse bg-white shadow-sm">
    <thead class="bg-gray-100">
      <tr>
        <th class="p-1 text-left">Category</th>
        <th class="p-1 text-right">Real</th>
        <th class="p-1 text-right">Synth</th>
        <th class="p-1 text-right">Diff</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="p-1 font-mono">Zeros (==0)</td>
        <td class="p-1 text-right">63.1%</td>
        <td class="p-1 text-right">39.9%</td>
        <td class="p-1 text-right text-red-500">-23.2%</td>
      </tr>
      <tr>
        <td class="p-1 font-mono">Noise (<0.2)</td>
        <td class="p-1 text-right">28.1%</td>
        <td class="p-1 text-right">48.8%</td>
        <td class="p-1 text-right text-green-500">+20.7%</td>
      </tr>
      <tr class="bg-gray-50 font-bold">
        <td class="p-1 border-t">SUM</td>
        <td class="p-1 text-right border-t">91.3%</td>
        <td class="p-1 text-right border-t">88.8%</td>
        <td class="p-1 text-right border-t">~ Equal</td>
      </tr>
    </tbody>
  </table>
  <div class="mt-2 text-[10px] leading-tight text-gray-600">
    The Insight: VAE outputs <em>noise</em> instead of <em>zeros</em>. Z-scoring (Standard View) amplifies this low-level noise, hiding the structure.
  </div>
</div>

</div>

<div v-click="3" class="mt-4 text-center bg-green-50 p-2 rounded text-sm text-green-800 font-bold">
  Conclusion: The model works perfectly. We just needed to fix the visualization.
</div>

---
layout: center
---

<img src="./resources/proper_normalization_run/Unconditional_overlay_denoised_scaled.png" class="h-80 object-contain rounded shadow-lg" />

---

## Run 2: Conditional Generation
### High-Fidelity Results across Cell Types

<div class="grid grid-cols-3 gap-x-4 gap-y-2 mt-4">
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_HSC_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">HSC</span>
  </div>
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_B_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">B Cells</span>
  </div>
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_T_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">T Cells</span>
  </div>
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_Mono-like_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">Mono-like (Malignant)</span>
  </div>
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_earlyEry_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">Early Erythroblasts</span>
  </div>
  <div class="flex flex-col items-center">
    <img src="./resources/proper_normalization_run/unscaled/conditional_umap_pDC_unscaled_vanilla.png" class="h-28 rounded shadow" />
    <span class="text-[10px] mt-1 font-bold">pDC</span>
  </div>
</div>

<div class="mt-6 bg-teal-50 p-3 rounded-lg border-l-4 border-teal-500 text-sm">
  <strong>The Proof:</strong> By conditioning on cell type labels, we can generate synthetic cells that perfectly match the biological manifold of their real counterparts. Note the preservation of <em>shape</em> and <em>density</em>.
</div>

---
layout: full
---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-3xl font-bold mb-4 text-teal-700">T Cell Detail: Run 2 (Unscaled)</h2>
  <img src="./resources/proper_normalization_run/unscaled/conditional_umap_T_unscaled_vanilla.png" class="h-[80%] rounded-xl shadow-2xl border-4 border-teal-500/30" />
</div>

---
layout: full
---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-3xl font-bold mb-4 text-purple-700">pDC Detail: Run 2 (Unscaled)</h2>
  <img src="./resources/proper_normalization_run/unscaled/conditional_umap_pDC_unscaled_vanilla.png" class="h-[80%] rounded-xl shadow-2xl border-4 border-purple-500/30" />
</div>

---

## Cell Type Dictionary
### Mapping Legend Keys to Biological Meaning

<div class="mt-4 overflow-hidden rounded-lg border border-gray-200 shadow-sm">
  <table class="w-full text-left text-sm">
    <thead class="bg-gray-50 font-bold text-gray-700">
      <tr>
        <th class="px-4 py-2 border-b">Key</th>
        <th class="px-4 py-2 border-b">Meaning</th>
        <th class="px-4 py-2 border-b">Key</th>
        <th class="px-4 py-2 border-b">Meaning</th>
      </tr>
    </thead>
    <tbody class="text-gray-600">
      <tr>
        <td class="px-4 py-1 font-mono text-teal-600 border-b">HSC</td>
        <td class="px-4 py-1 border-b">Hematopoietic Stem Cells</td>
        <td class="px-4 py-1 font-mono text-red-600 border-b">Ery</td>
        <td class="px-4 py-1 border-b">Red Blood Cell Precursors</td>
      </tr>
      <tr class="bg-gray-50/50">
        <td class="px-4 py-1 font-mono text-teal-600 border-b">Prog</td>
        <td class="px-4 py-1 border-b">Progenitors</td>
        <td class="px-4 py-1 font-mono text-blue-600 border-b">B / T / NK</td>
        <td class="px-4 py-1 border-b">Immune Cells (Lymphoid)</td>
      </tr>
      <tr>
        <td class="px-4 py-1 font-mono text-orange-600 border-b">GMP</td>
        <td class="px-4 py-1 border-b">Granulocyte Monocyte Progenitors</td>
        <td class="px-4 py-1 font-mono text-blue-600 border-b">CTL</td>
        <td class="px-4 py-1 border-b">Cytotoxic T-Cells</td>
      </tr>
      <tr class="bg-gray-50/50">
        <td class="px-4 py-1 font-mono text-orange-600 border-b">Mono</td>
        <td class="px-4 py-1 border-b">Monocytes</td>
        <td class="px-4 py-1 font-mono text-purple-600 border-b">pDC / cDC</td>
        <td class="px-4 py-1 border-b">Dendritic Cells</td>
      </tr>
      <tr class="bg-yellow-50 font-bold">
        <td class="px-4 py-2 font-mono text-gray-900">*-like</td>
        <td class="px-4 py-2 text-gray-900" colspan="3">Malignant / Leukemic Cells (e.g. Mono-like = AML Monocytes)</td>
      </tr>
    </tbody>
  </table>
</div>

---

## Global Manifold Reconstruction: Real vs Synthetic
### Unscaled Data (Run 2)

<div class="grid grid-cols-2 gap-8 mt-4">
  <div class="flex flex-col items-center">
    <h3 class="font-bold text-teal-700 mb-2 text-xl">Real Data</h3>
    <img src="./resources/proper_normalization_run/unscaled/umap_all_real_with_legend.png" class="h-80 rounded shadow-lg border-2 border-teal-500/50" />
    <p class="mt-4 text-sm text-gray-600 font-semibold text-center px-4">
      The Ground Truth biological manifold from the Van Galen dataset.
    </p>
  </div>
  
  <div class="flex flex-col items-center">
    <h3 class="font-bold text-purple-700 mb-2 text-xl">Synthetic Data</h3>
    <img src="./resources/proper_normalization_run/unscaled/umap_all_synthetic_with_legend.png" class="h-80 rounded shadow-lg border-2 border-purple-500/50" />
    <p class="mt-4 text-sm text-gray-600 font-semibold text-center px-4">
      Generated cells (conditioned on label) reconstruct the manifold structure.
    </p>
  </div>
</div>

---
layout: section
---

# IV. Outlook & Discussion

---

## Next Steps

<div class="grid grid-cols-6 gap-8 mt-16 px-8">

<div v-click="1" class="col-span-2 flex items-center gap-4">
  <div class="bg-indigo-100 p-3 rounded-full">
    <carbon:compare class="text-2xl text-indigo-600" />
  </div>
  <div>
    <h3 class="font-bold text-lg">scimilarity Integration</h3>
    <p class="text-sm text-gray-600">Leverage scimilarity to improve cell type conditioning and representation.</p>
  </div>
</div>

<div v-click="2" class="col-span-2 flex items-center gap-4">
  <div class="bg-blue-100 p-3 rounded-full">
    <carbon:layers class="text-2xl text-blue-600" />
  </div>
  <div>
    <h3 class="font-bold text-lg">Full Dataset Run</h3>
    <p class="text-sm text-gray-600">Scale up from HVGs to the full transcriptome to capture all biological signals.</p>
  </div>
</div>

<div v-click="3" class="col-span-2 flex items-center gap-4">
  <div class="bg-purple-100 p-3 rounded-full">
    <carbon:flow-stream class="text-2xl text-purple-600" />
  </div>
  <div>
    <h3 class="font-bold text-lg">Interpolation</h3>
    <p class="text-sm text-gray-600">Map the continuous developmental trajectories (e.g. HSC -> Erythroid).</p>
  </div>
</div>

<div v-click="4" class="col-span-3 flex items-center gap-4">
  <div class="bg-teal-100 p-3 rounded-full">
    <carbon:split-screen class="text-2xl text-teal-600" />
  </div>
  <div>
    <h3 class="font-bold text-lg">Multiconditional</h3>
    <p class="text-sm text-gray-600">Generate complex states (e.g. CellType + Disease State).</p>
  </div>
</div>

<div v-click="5" class="col-span-3 flex items-center gap-4">
  <div class="bg-yellow-100 p-3 rounded-full">
    <carbon:chart-evaluation class="text-2xl text-yellow-600" />
  </div>
  <div>
    <h3 class="font-bold text-lg">Rigorous Evaluation</h3>
    <p class="text-sm text-gray-600">Leave-one-out cross-validation to prove generalization capabilities.</p>
  </div>
</div>

</div>