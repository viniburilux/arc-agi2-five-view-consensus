# Five-view consensus for ARC-AGI-2

Vinícius Buri. Scientific disclosure companion to *When Five Views Agree: Early-Exit Consensus and the ARC-AGI-2 Generalization Gap*.

## What is released

`scientific_source.py` contains the audited scientific inference, view construction, decoding, cache verification, and selection modules from the private Giotto V4 candidate. It is not the original official notebook or an end-to-end runnable replacement. The original candidate's public leaderboard score was 1.67% (submission 57753239). This disclosure has no official score.

The public Giotto source is from https://github.com/giotto-ai/giotto-arc-agi-model and is AGPL-3.0. The bundled derivative is distributed under AGPL-3.0, with the full license provided. Giotto's authors retain their upstream rights. The weights/tokenizer are separately available at https://zenodo.org/records/18612211 under CC0-1.0; they are not mirrored here. Technical report: https://arxiv.org/html/2603.06590v1.

## Differences from the official candidate

Comments and docstrings were removed and formatting normalized. For each bundled module, the executable AST was checked equal before and after this transformation. Original and disclosed SHA-256 values are in the embedded provenance manifest. The archive excludes historical preparation notes, launch wrappers, test archives, outputs, and privately hosted dependencies. The original artifact remains private.

Optional TTT modules are not bundled. TTT was OFF in the official candidate and is unsupported here. Do not set the generation callback's `ttt` argument or supply adaptation hooks. Inference requires compatible numpy, tokenizers, transformers, torch, huggingface_hub and tqdm, and a locally supplied verified cache. Historical default cache paths are not public dependencies: pass a cache root explicitly and set ARC_CACHE before importing runtime_adapter.

## Evidence and limitations

400 synthetic CPU comparisons against the original view/selection modules matched normalized result fields (excluding elapsed seconds), synthetic raw arrays, abort status, and both serializers. They used synthetic callbacks and a test decoder, not a real model. No checkpoint load, tokenizer validation, GPU inference, complete pipeline, or official score was rerun on this release. AST checks do not test environmental or optional-dependency behavior.

The original panel comprised 27 leave-one-out demonstration folds, not independent unseen tasks. Selection changed 21 to 24 folds solved. Separately, recorded-token replay preserved decisions on 27/27 folds and used 93 versus 135 generations. Agreement is not calibrated confidence. The public-evaluation result of 169/172 with two attempts did not transfer to the official 1.67% score; the cause remains unresolved.

## Inspecting the code

Extract into an empty directory with `python scientific_source.py --extract source`. This only writes source files, not model execution. `candidate.py`, `vote_solver.py`, and `make_view.py` contain selection, early exit, and view transforms. `runtime_adapter.py` and `decode_contract.py` handle canonical encoding/decoding. `generation_callback.py` contains the deterministic GPU generation settings and absolute deadline checks. It requires an isolated process group and explicit future deadline; this release does not automatically launch it. `giotto_arc_agi_model/` is the disclosed upstream inference source with the modifications documented in the paper and manifest.

The mathematical illustration notebook linked from the paper is newly authored, separate code under Kaggle's Apache-2.0 terms. It is not this AGPL source and cannot alone reproduce the official solver.
