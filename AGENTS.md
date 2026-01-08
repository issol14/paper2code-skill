# AGENTS.md - For LLM Agents

> This file is specifically designed for LLM agents (Claude, GPT, etc.) to quickly understand and work with this repository.

## QUICK SUMMARY

**paper2code-skill** is a Claude Code Skill that converts research papers (PDF/arXiv) into executable code using a structured 4-phase pipeline.

## REPOSITORY STRUCTURE

```
paper2code-skill/
├── README.md                 # Human-readable documentation
├── AGENTS.md                 # This file - for LLM agents
├── LICENSE                   # MIT License
├── CONTRIBUTING.md           # Contribution guidelines
├── .gitignore
└── skills/
    └── paper2code/           # ← The actual skill files
        ├── SKILL.md                      # Main entry point
        ├── 01_algorithm_extraction.md    # Phase 1
        ├── 02_concept_analysis.md        # Phase 2
        ├── 03_code_planning.md           # Phase 3
        ├── 04_implementation_guide.md    # Phase 4
        ├── 05_reference_search.md        # Phase 0 (optional)
        ├── 06_memory_management.md       # Memory guide
        └── README.md                     # Skill-specific docs
```

## INSTALLATION COMMANDS

```bash
# One-liner installation
git clone https://github.com/issol14/paper2code-skill.git /tmp/paper2code-skill && \
cp -r /tmp/paper2code-skill/skills/paper2code ~/.claude/skills/ && \
rm -rf /tmp/paper2code-skill && \
echo "✅ Installed successfully"
```

### Verification
```bash
ls ~/.claude/skills/paper2code/SKILL.md && echo "✅ Skill is ready"
```

## WHAT THIS SKILL DOES

When a user provides a research paper (PDF or arXiv URL), this skill guides Claude through:

| Phase | File | Purpose |
|-------|------|---------|
| 0 (opt) | `05_reference_search.md` | Find similar implementations on GitHub |
| 1 | `01_algorithm_extraction.md` | Extract algorithms, equations, hyperparameters |
| 2 | `02_concept_analysis.md` | Analyze paper structure, components, experiments |
| 3 | `03_code_planning.md` | Create detailed implementation plan (YAML) |
| 4 | `04_implementation_guide.md` | Generate code file-by-file |

## KEY BEHAVIORAL RULES

```
DO:
✓ Implement exactly what the paper specifies
✓ One file at a time, no permission asking between files
✓ Save intermediate YAML outputs
✓ Run self-check before completing each phase

DON'T:
✗ Ask "Should I continue?" between files
✗ Guess parameter values not in the paper
✗ Skip unclear parts (document in missing_but_critical)
✗ Over-engineer or add unnecessary abstractions
```

## ACTIVATION TRIGGERS

The skill activates when users say things like:
- "이 논문 구현해줘" (Implement this paper)
- "paper2code"
- "논문 코드로 변환" (Convert paper to code)
- "implement this paper"
- Provide an arXiv URL or PDF path

## OUTPUT FORMAT

Each phase produces YAML output saved to files:
```
paper_workspace/
├── 01_algorithm_extraction.yaml
├── 02_concept_analysis.yaml
├── 03_implementation_plan.yaml
└── src/
    ├── config.py
    ├── models/
    ├── algorithms/
    ├── training/
    ├── evaluation/
    ├── main.py
    └── README.md
```

## FOR AGENTS HELPING WITH INSTALLATION

If a user asks you to install this skill:

1. **Check if Claude Code is available** (this skill is for Claude Code)
2. **Run the installation commands** shown above
3. **Verify** the SKILL.md file exists
4. **Inform the user** they can now use paper implementation commands

## COMMON ISSUES

| Issue | Solution |
|-------|----------|
| Permission denied | Ensure `~/.claude/skills/` directory exists and is writable |
| Skill not activating | Check file names are exact, especially `SKILL.md` |
| PDF not readable | Install `poppler-utils` for pdftotext support |

## LINKS

- Repository: https://github.com/issol14/paper2code-skill
- Claude Code: https://claude.ai/code
- Agent Skills Standard: https://agentskills.io
