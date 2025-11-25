# Workshop on Benchmarking for LLMs in Software Engineering
## Comprehensive Summary

---

## Executive Summary

This workshop focused on creating realistic, dynamic, and high-quality benchmarks for evaluating Large Language Models (LLMs) in software engineering contexts. The primary infrastructure discussed was **World of Code**, which aggregates over 5 billion commits from open source repositories to enable provenance tracking and quality assurance in benchmark creation.

Key findings revealed significant quality issues in current training datasets (Stack v2): **17% of files contain known bugs**, **2% have CVEs**, and **50-60% are never-modified code** (likely toy/unused projects). The workshop emphasized that static benchmarks suffer from data leakage and contamination, making dynamic generation from live repositories essential.

---

## Workshop Aims

The workshop identified eight primary objectives:

1. **Build realistic and dynamic benchmarks** using real-world software repositories
2. **Minimize data leakage and contamination** in benchmark datasets
3. **Reduce manual effort** in benchmark creation through automation and tooling
4. **Improve data quality** by removing bugs and vulnerabilities from training data
5. **Create infrastructure** for continuous benchmark generation and updating
6. **Develop benchmarks for entire SDLC** (not just isolated code generation tasks)
7. **Establish community standards** and shared tools for benchmark creation
8. **Address evaluation challenges** for agentic and multi-agent systems

---

## Key Actions Discussed

### Infrastructure Development

1. **Use World of Code** to trace file history and identify fixed bugs/vulnerabilities
2. **Leverage version control metadata** to ensure benchmark quality
3. **Create automated workflows** for benchmark generation from live repositories
4. **Establish shared platforms** for benchmark distribution (analogous to Hugging Face)

### Tooling and Automation

5. **Develop annotation tools** to reduce manual effort (inspired by Scale AI approach)
6. **Build benchmark workbenches/DSLs** for semi-automated creation
7. **Implement LLM-based agents** for automated benchmark review and validation
8. **Create template benchmarks** and standardized evaluation frameworks

### Quality Assurance

9. **Develop tools for semantic deduplication** and diversity analysis
10. **Use reinforcement learning approaches** (like SweetRL) for training on real PRs
11. **Implement multi-stage evaluation** with intermediate milestone tracking

---

## Most Important Conclusions

### Technical Conclusions

1. **World of Code (5B+ commits)** provides infrastructure to build higher-quality benchmarks by tracing provenance and software supply chains

2. **Current training datasets contain significant quality issues:**
   - Stack v2 full dataset: 17% files with known bugs that were later fixed
   - 2% of files contain known CVEs (security vulnerabilities)
   - 50-60% of files have no change history (likely unused/toy code)
   - ~65% have old versions but no newer versions (abandoned code)

3. **Small benchmarks can be effective** - 200 examples sufficient as "smoke tests" for detecting serious bugs, though not comprehensive enough for fine-tuning

4. **Data leakage cannot be solved by temporal filtering alone** - requires full software supply chain analysis due to code copying across projects

5. **Different benchmark types needed:**
   - Smoke tests (detect serious bugs with small samples)
   - Comprehensive evaluation benchmarks
   - Fine-tuning datasets with diverse examples

6. **Test-passing is insufficient metric** - need intent-based evaluation recognizing multiple valid solutions to the same problem

### Process Conclusions

1. **Benchmark creation requires 50-75% of research effort** in papers introducing new techniques - this overhead is unsustainable

2. **Manual benchmark creation does not scale** - automation essential but must preserve quality through human validation

3. **Community needs shared infrastructure** - tools, templates, platforms to avoid redundant effort across research groups

4. **Semi-automated approach most promising** - combining LLM agents for generation with human validation for quality control

5. **Benchmarks must evolve continuously** - static benchmarks inevitably suffer contamination and lose relevance

### Methodological Conclusions

1. **Static benchmarks are inherently problematic** due to contamination from training data and limited scope

2. **Dynamic generation from live repositories** (SweetBench Live, OSS Bench) is necessary direction, though still vulnerable to leakage from code copying

3. **Software engineering context makes benchmarks more realistic** - using real bug fixes, PRs, and development processes rather than synthetic problems

4. **Benchmarks should incorporate full SDLC** - not just isolated code generation, but also testing, documentation, requirements, code review

5. **Multi-agent and agentic systems require milestone-based evaluation** - not just binary success/failure, but tracking progress through intermediate steps

---

## Critical Research Questions Identified

### Three Priority Questions (from Satish)

1. **How to create better code editing and refactoring benchmarks** that are representative, live, and properly scored? (Currently underserved area)

2. **How to properly evaluate LLM performance** with intent-based scoring beyond simple test passing?

3. **How to benchmark modern AI-assisted development tools** (Cursor, Codex CLI, Gemini CLI, etc.) in realistic usage scenarios?

### Additional Open Questions

4. How to evaluate **personalized and continually-learning coding assistants** that adapt to developer style?

5. **Which benchmarks should researchers use** given proliferation of hundreds of similar options?

6. How to create benchmarks for **low-resource programming languages** (COBOL, Fortran) without sufficient training data?

7. **Can LLMs reliably generate their own benchmarks** without introducing bias or overfitting?

8. How to **help developers decide whether to accept LLM-generated code** - establishing trust signals?

9. Should we have **general benchmarks or adaptive/personalized benchmarks** tailored to specific contexts?

---

## Major Technical Findings

### World of Code Infrastructure

**World of Code** collects and organizes all public version control data:

- **5+ billion commits** from all open source projects
- **26 billion trees** (directory structure snapshots)
- **26 billion blobs** (file versions)
- **300 million project URLs** monitored (GitHub, Bitbucket, Hugging Face, etc.)
- Only **~30 million projects have updates per quarter** (most are inactive)

**Key capabilities:**
- Trace any file blob to all projects that use it
- Identify first project to create a file (for licensing)
- Track bug fixes and vulnerability patches across ecosystem
- Detect code copying and supply chain relationships
- Extract APIs and study ecosystem adoption patterns

### Stack v2 Quality Analysis

Analysis of Stack v2 training dataset using World of Code revealed:

| Issue Type | Full Dataset | Small Dataset | Implication |
|------------|--------------|---------------|-------------|
| Files with later bug fixes | 17% | 14% | Training on buggy code |
| Files with CVEs | 2% | Unknown | Training on vulnerabilities |
| Files never changed | 50-60% | 50-60% | Likely toy/unused code |
| Files with old but no new version | ~65% | ~58% | Abandoned/incomplete code |

**Recommendation:** Use World of Code to replace buggy versions with fixed versions in training data, and filter out never-modified files.

### Benchmark Families Discussed

| Benchmark | Purpose | Limitation |
|-----------|---------|------------|
| **SweetBench** | Code generation from GitHub issues (12 projects) | Static, limited scope, data leakage |
| **SweetBench Live** | Dynamic benchmark from recent PRs | Still vulnerable to copied code |
| **SweetBench Multimodal** | Multi-language extensions | Limited language coverage |
| **OSS Bench** | Similar to SweetBench Live | Manual effort required |
| **Human Eval** | Simple function-level generation | Too easy, not challenging |
| **SecBench** | Security vulnerability detection/fixing | Fully automated but limited scope |
| **GEVBench** | Efficiency/performance benchmarking | Discussed but details unclear |
| **AppBench** | Web application generation | Personalized evaluation difficult |

---

## Project Examples and Techniques

### SweetRL Project

**Approach:** Reinforcement learning for code generation
- Give model issue descriptions from real GitHub projects
- Model generates thousands of possible PR solutions
- Use rule-based rewards (e.g., syntactic similarity to ground truth PR)
- Model learns from self-exploration without human annotation
- Generalizes better than supervised fine-tuning (SFT)

**Challenge:** Potential overfitting to specific ground truth patterns when only one reference solution exists

### SecBench Automation

**Approach:** Fully automated security benchmark creation using agents
- Agent automatically finds security vulnerabilities
- Downloads and compiles projects
- Fixes compilation errors automatically
- Runs tests and fixes test failures
- Handles missing dependencies
- Creates reproducible Docker images

**Result:** 75% of paper effort was benchmark creation, fully automated

### Scale AI Annotation Platform

**Commercial approach to benchmark creation:**
- Provides infrastructure for both training data and annotation
- Uses graduate students or professionals to annotate small samples (e.g., 1,000 images)
- Trains agent on annotated samples
- Validates agent accuracy against human annotators
- Scales to millions of annotations once validated
- **Business model:** Companies pay for annotation services

**Academic lesson:** Small high-quality human annotations + AI scaling = cost-effective benchmark creation

---

## Recommendations for Community

### Infrastructure Recommendations

1. **Create "Hugging Face for Benchmarks"** - centralized platform for sharing:
   - Benchmark datasets with standardized formats
   - Annotation tools and workflows
   - Evaluation scripts and metrics
   - Template benchmarks for common tasks

2. **Develop benchmark workbenches/DSLs** for semi-automated creation:
   - Specify task requirements in structured format
   - Automatically generate Docker containers
   - Set up execution environments
   - Configure metrics and evaluation

3. **Establish annotation tools** with features:
   - Inter-rater reliability tracking
   - Quality control workflows
   - Integration with LLM-based pre-annotation
   - Graduate student assignment and grading integration

### Methodological Recommendations

4. **Build semantic deduplication tools** to ensure:
   - Diversity in benchmark examples
   - Detection of copied code across projects
   - Prevention of overfitting to specific patterns

5. **Create guidelines/standards** for benchmark quality:
   - What makes a benchmark "good"?
   - How to measure representativeness?
   - How to ensure reproducibility?
   - How to document provenance?

6. **Foster specialized benchmarks** rather than one-size-fits-all:
   - Code generation vs editing vs refactoring
   - Different programming languages
   - Different difficulty levels
   - Different evaluation contexts (education vs production)

### Technical Recommendations

7. **Leverage World of Code** for:
   - Provenance tracking to avoid contamination
   - Bug/vulnerability detection and filtering
   - License compliance verification
   - Diversity analysis across projects

8. **Use reinforcement learning approaches** like SweetRL:
   - Learn from real development patterns
   - Generate multiple solution strategies
   - Reward realistic solutions, not just test-passing

9. **Implement milestone-based evaluation** for agentic systems:
   - Track progress through development stages
   - Reward partial solutions
   - Evaluate reasoning and planning, not just final output

---

## Discussion Highlights

### On Benchmark Size

**Surprising finding:** Small benchmarks (200 examples) can be highly effective as "smoke tests"

**Explanation from participants:**
- If models have serious bugs, small samples will reveal them
- Similar to statistical significance: large effect sizes detectable with small samples
- Election analogy: Close races need large samples, landslides don't
- **Not useful for:** Fine-tuning or comprehensive evaluation
- **Useful for:** Detecting catastrophic failures before deployment

### On Fine-Tuning vs Context Windows

**Debate:** As context windows grow larger, is fine-tuning still necessary?

**Arguments for "fine-tuning becoming obsolete":**
- Can include many examples in context (few-shot learning)
- RAG can retrieve relevant information
- Easier and cheaper than full fine-tuning

**Arguments for "fine-tuning still valuable":**
- Best way to prime model for specific tasks
- Updates model weights, not just context
- Parameter-efficient fine-tuning (PEFT) makes it affordable
- In-context learning can cause models to "lose focus" with too much context

**Consensus:** Fine-tuning for task-specific behavior, RAG/context for incorporating project-specific knowledge

### On Human Evaluation

**Challenge:** Human evaluation is gold standard but expensive and non-repeatable

**Novel idea proposed:** Use benchmarks for student grading
- Give students same benchmark tasks as LLMs
- Grade based on inter-rater reliability with professional annotators
- Higher agreement with professionals = higher grade
- Motivates students to produce high-quality annotations
- Generates large amounts of human annotation data

**Benefit:** Combines education with research data collection

### On Data Leakage

**Key insight:** Temporal filtering (SweetBench Live) is insufficient

**Reasons:**
- Code is frequently copied between projects
- Same patterns appear in different contexts
- PR may be "new" but code patterns are well-known
- Need full software supply chain analysis, not just timestamps

**World of Code solution:**
- Trace every blob to first appearance
- Identify all projects using that code
- Detect copying relationships
- Filter based on semantic similarity, not just time

---

## Future Directions

### Short-term Actions

1. **Create working group** to develop benchmark infrastructure proposal
2. **Draft vision paper** on community standards for benchmarking
3. **Prototype annotation tools** with inter-rater reliability
4. **Extend World of Code analysis** to more training datasets

### Medium-term Goals

5. **Launch "Benchmark Hub"** platform for sharing
6. **Develop benchmark workbench** prototype
7. **Create specialized benchmarks** for underserved areas:
   - Code editing and refactoring
   - Requirements and documentation
   - Multi-agent collaboration
8. **Establish evaluation standards** for agentic systems

### Long-term Vision

9. **Continuous benchmark generation** from live repositories
10. **Intent-based evaluation** recognizing multiple valid solutions
11. **Personalized benchmarks** adapting to developer context
12. **Feedback loops** from deployed tools to improve benchmarks
13. **Cross-language benchmarks** for low-resource languages

---

## Conclusion

The workshop highlighted that **benchmarking for LLMs in software engineering is at a critical juncture**. While recent advances like SweetBench represent progress toward realistic evaluation, significant challenges remain:

- **Quality:** Training data contains bugs, vulnerabilities, and unused code
- **Leakage:** Static benchmarks inevitably contaminate training data
- **Effort:** Manual creation consumes 50-75% of research effort
- **Scope:** Current benchmarks focus narrowly on code generation, neglecting editing, refactoring, and full SDLC
- **Evaluation:** Test-passing fails to capture intent and multiple valid solutions

**The path forward requires community collaboration** to build shared infrastructure, automate benchmark creation while maintaining quality, and develop new evaluation paradigms for agentic systems. World of Code provides a promising foundation for provenance tracking and quality assurance, but significant work remains to realize the vision of dynamic, realistic, continuously-updated benchmarks that truly measure LLM capabilities in software engineering.

**Three critical research questions** must be addressed:
1. How to create realistic code editing/refactoring benchmarks?
2. How to perform intent-based evaluation beyond test-passing?
3. How to benchmark modern AI-assisted development tools in real usage?

Addressing these questions will require not just technical innovation, but also community coordination to avoid fragmentation and redundant effort across research groups.

---

## Appendix: Participants and Contributions

- **Satish (Industry Expert):** Identified three critical research questions; discussed deployment challenges and evaluation methods
- **World of Code Presenter:** Demonstrated infrastructure for benchmark quality analysis; revealed Stack v2 issues
- **Chris:** Raised questions about developer trust and acceptance of LLM suggestions
- **Michele & Juergen (Remote):** Discussed multi-agent systems and SDLC benchmark requirements
- **Workshop Participants:** Contributed questions on benchmark selection, personalization, low-resource languages, and automation

---

**Workshop Date:** Based on transcript content, appears to be recent academic workshop (2024-2025 timeframe)

**Key References Mentioned:**
- SweetBench and SweetBench Live papers
- Stack v2 dataset
- World of Code infrastructure
- SecBench security benchmark
- AppBench for web applications
- Scale AI annotation platform
