# NLProtein

Code for *NLProtein: A Biological Grounding-Aware Instruction Following Protein
Design Model* (under review, ICLR 2027).

The full code, dataset construction scripts, NLProteinBench, and model weights will be released on acceptance.

## Files

**`language_guided_protein_design_model.py`** 
The NLProtein model: a text encoder for natural-language function descriptions,
frozen ligand (ChemBERTa) and antigen (ESM-2) encoders injected through
zero-initialized gated cross-attention adapters, and a protein decoder
initialized from ProGen2-large-BFD90.

**`language_guided_protein_design_task.py`** 
The fairseq task: builds the model, loads the datasets, and performs
sequence generation during validation.

**`nlprotein_loss.py`** 
The training objective, `L = L_protein + 0.5 * L_text`, including the loss mask
that excludes the given prefix on span-infilling examples.

**`nlprotein_dataset.py`** 
Batches examples and pads each input stream with its own encoder's pad token.

**`indexed_dataset.py`** 
Reads the JSONL and packed-binary data files for Stage 1 pretraining and for
the Stage 2 ligand and antibody datasets.

**`progen_vocab.py`** 
The amino-acid alphabet, including the `<SEP>` and `<M1>`-`<M30>` tokens used
for span infilling in both stages.

## Architecture modes

`--architecture-mode` selects which components are trainable: `stage1` for
pretraining, and `stage2` for the biological grounding stage reported in the
paper. `stage2_frozen_decoder`, `stage2_partial` and `stage2_adapters_only` are
ablations.
