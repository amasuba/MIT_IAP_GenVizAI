# Agentic Self-Debugging Workflow (Continuous Loop)

## Overview
This workflow implements a continuous, agentic self-debugging loop for messy codebases, aligning with the project's hypothesis: KG-augmented agents (Method B) outperform vector embeddings (Method A) in fixing bugs via relational context. Built with CrewAI for orchestration and NetworkX for KG. The loop detects bugs, proposes/simulates fixes (A/B), validates/applies, and monitors for new issues—proactive via RL simulations.

**Key Components**:
- **Agents**: Planner, ContextFetcher (A/B: embeddings/KG), BugDetector, FixProposer, RLSimulator, Validator, Monitor.
- **Loop**: Detect → Propose (A/B) → Simulate (RL) → Validate/Apply → Monitor → Repeat if bugs persist.
- **Tech**: CrewAI (orchestration), NetworkX (KG), FAISS/UMAP (embeddings), Ray RLlib/Gymnasium (RL sims), Pydantic (state models).
- **Integration**: Uses Sonny's KnowledgeGraph (embeddings from graph.py); Rob's messy codebase (e.g., inputs/simpleCalculator).
- **A/B Testing**: Embedded in ContextFetcher/FixProposer; metrics logged for comparison.

## Setup
1. Install deps: `pip install -r requirements.txt`
2. Config: Edit `config.json` (e.g., codebase_path: "inputs/simpleCalculator")
3. Run: `python workflow.py` (starts continuous loop; Ctrl+C to stop)

## Continuous Loop Flow
1. **Monitor**: Checks for bugs/code changes (e.g., via git diff or static analysis).
2. **Detect**: BugDetector scans for issues (races, deadlocks in multi-process).
3. **Context**: ContextFetcher gets relevant code (A: embeddings search; B: KG traversal).
4. **Propose**: FixProposer generates patches (A/B variants).
5. **Simulate**: RLSimulator tests fixes proactively (rewards safe, efficient outcomes).
6. **Validate/Apply**: Validator runs tests; applies if safe.
7. **Back to Monitor**: Loop if issues remain (threshold-based).

## Agents Overview
- See `agents.py` for definitions.

## Usage Example
```bash
python workflow.py --codebase inputs/simpleCalculator/ --loop-interval 60  # Run every 60s
