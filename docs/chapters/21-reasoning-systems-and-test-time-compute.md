# Chapter 21: Reasoning Systems and Test-Time Compute

## The Art of Machine Thought

For decades, artificial intelligence was measured by its answers. We asked questions and judged the quality of responses. Speed mattered. Accuracy mattered. But the process—the invisible dance of reasoning that leads from question to answer—remained hidden, compressed into a single forward pass through frozen weights.

This is changing. We are entering the era of reasoning models: systems that think step by step, that explore multiple paths, that verify their own conclusions, that allocate compute to the thinking process itself. These systems do not merely produce answers—they produce chains of thought, verifiable reasoning, cognitive trails that can be followed, audited, and improved.

This chapter explores the architecture of machine reasoning: chain-of-thought prompting, tree of thoughts, self-consistency, and the revolutionary paradigm of test-time compute scaling. This is the engineering of cognition itself.

---

## What Is Reasoning in AI?

### Beyond Pattern Matching

Traditional language models are sophisticated pattern matchers. They predict the next token based on statistical patterns learned during training. This produces remarkable capabilities but has limitations:

- **No explicit step-by-step thinking**: The reasoning is implicit in the weights, not explicit in the output
- **No self-correction**: Cannot recognize and fix errors in mid-generation
- **No exploration**: Cannot consider multiple approaches and select the best
- **Fixed compute**: Every question gets the same processing, regardless of difficulty

**Reasoning models** address these limitations by making the thinking process explicit, explorable, and scalable.

### Types of Machine Reasoning

```
┌─────────────────────────────────────────────────────────────────┐
│                   REASONING PARADIGMS                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  CHAIN-OF-      │  │   TREE OF       │  │   SELF-         │ │
│  │  THOUGHT        │  │   THOUGHTS      │  │   CONSISTENCY   │ │
│  │                 │  │                 │  │                 │ │
│  │  Linear chain   │  │  Branching      │  │  Multiple       │ │
│  │  of reasoning   │  │  exploration    │  │  paths,         │ │
│  │  steps          │  │  of solution    │  │  majority       │ │
│  │                 │  │  space          │  │  voting         │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  VERIFICATION   │  │   PROGRAM-      │  │   TEST-TIME     │ │
│  │  & REFINEMENT   │  │   AIDED         │  │   COMPUTE       │ │
│  │                 │  │                 │  │                 │ │
│  │  Check and      │  │  Generate       │  │  Scale thinking │ │
│  │  correct own    │  │  executable     │  │  time with      │ │
│  │  reasoning      │  │  code           │  │  problem        │ │
│  │                 │  │                 │  │  difficulty     │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Chain-of-Thought: Making Thinking Explicit

### The Core Idea

Chain-of-Thought (CoT) prompting elicits reasoning by asking the model to show its work. Instead of jumping directly to an answer, the model generates intermediate reasoning steps.

**Standard Prompting**:
```
Q: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. 
   Each can has 3 balls. How many does he have now?
A: 11
```

**Chain-of-Thought Prompting**:
```
Q: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. 
   Each can has 3 balls. How many does he have now?
A: Roger starts with 5 balls. 2 cans of 3 balls each is 6 balls.
   5 + 6 = 11. The answer is 11.
```

### Implementation: Zero-Shot CoT

```python
async def zero_shot_cot(llm, question: str) -> str:
    """
    Zero-shot chain-of-thought with a simple trigger phrase.
    """
    prompt = f"""Question: {question}

Let's think step by step and show the reasoning process.

Answer:"""
    
    response = await llm.generate(prompt)
    return response.content

# Example usage
question = """A juggler has 16 balls. Half are golf balls. 
Half of the golf balls are blue. How many blue golf balls are there?"""

response = await zero_shot_cot(llm, question)
# Output: First, I need to find how many are golf balls: 16 / 2 = 8 golf balls.
#         Then, half of those are blue: 8 / 2 = 4 blue golf balls.
#         The answer is 4.
```

### Implementation: Few-Shot CoT with Examples

```python
FEW_SHOT_COT_EXAMPLES = """Q: There are 15 trees in the grove. Grove workers will plant trees today. 
After they are done, there will be 21 trees. How many trees did they plant?
A: There are 15 trees originally. After planting, there are 21 trees. 
The difference is 21 - 15 = 6 trees planted. The answer is 6.

Q: If there are 3 cars in the parking lot and 2 more cars arrive, 
how many cars are in the parking lot?
A: There are originally 3 cars. 2 more arrive. 3 + 2 = 5 cars. The answer is 5.

Q: Leah had 32 chocolates and her sister had 42. If they ate 35, 
how many pieces do they have left in total?
A: Leah had 32 and her sister had 42. Together they had 32 + 42 = 74.
They ate 35, so 74 - 35 = 39 remain. The answer is 39."""

async def few_shot_cot(llm, question: str) -> str:
    """
    Few-shot chain-of-thought with example reasoning patterns.
    """
    prompt = f"""{FEW_SHOT_COT_EXAMPLES}

Q: {question}
A:"""
    
    response = await llm.generate(prompt)
    return response.content
```

### Advanced CoT: Self-Consistency

Generate multiple reasoning paths and take the most common answer:

```python
import asyncio
from collections import Counter

async def self_consistency_cot(
    llm, 
    question: str, 
    num_paths: int = 10,
    temperature: float = 0.7
) -> dict:
    """
    Generate multiple CoT reasoning paths and return most consistent answer.
    """
    
    async def generate_single_path() -> str:
        """Generate one reasoning path with sampling."""
        prompt = f"""Question: {question}

Let's work through this step by step:

Solution:"""
        
        response = await llm.generate(
            prompt, 
            temperature=temperature,
            max_tokens=500
        )
        return response.content
    
    # Generate multiple paths in parallel
    paths = await asyncio.gather(*[
        generate_single_path() for _ in range(num_paths)
    ])
    
    # Extract final answers (simplified extraction)
    answers = []
    for path in paths:
        # Look for "The answer is X" pattern
        lines = path.split('\n')
        for line in reversed(lines):
            if 'answer is' in line.lower():
                # Extract number or phrase after "answer is"
                answer = line.lower().split('answer is')[-1].strip('. ')
                answers.append(answer)
                break
        else:
            answers.append(path[-50:])  # Last 50 chars as fallback
    
    # Count frequencies
    answer_counts = Counter(answers)
    most_common = answer_counts.most_common(1)[0]
    
    return {
        "answer": most_common[0],
        "confidence": most_common[1] / num_paths,
        "all_paths": paths,
        "answer_distribution": dict(answer_counts)
    }

# Usage
result = await self_consistency_cot(llm, complex_math_question, num_paths=20)
print(f"Answer: {result['answer']} (confidence: {result['confidence']:.1%})")
```

---

## Tree of Thoughts: Branching Exploration

### When Linear Reasoning Fails

Some problems require exploring multiple possibilities:
- Creative writing with multiple valid directions
- Strategic games with branching moves
- Complex planning with alternative approaches
- Puzzle solving with uncertain intermediate steps

Tree of Thoughts (ToT) maintains multiple reasoning paths simultaneously, exploring the solution space like a search algorithm.

### ToT Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    TREE OF THOUGHTS                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                      ┌─────────────┐                             │
│                      │  Problem  │                             │
│                      └──────┬────┘                             │
│                             │                                   │
│            ┌────────────────┼────────────────┐                 │
│            │                │                │                 │
│            ▼                ▼                ▼                 │
│      ┌─────────┐      ┌─────────┐      ┌─────────┐            │
│      │Thought 1│      │Thought 2│      │Thought 3│            │
│      │ 7/10    │      │ 4/10    │      │ 8/10    │            │
│      └────┬────┘      └────┬────┘      └────┬────┘            │
│           │                │                │                  │
│     ┌─────┴─────┐          │          ┌─────┴─────┐            │
│     │           │          │          │           │            │
│     ▼           ▼          ▼          ▼           ▼            │
│ ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐         │
│ │Child 1│  │Child 2│  │Dead   │  │Child 3│  │Child 4│         │
│ │ 6/10  │  │ 9/10  │  │ 2/10  │  │ 7/10  │  │ 8/10  │         │
│ └───────┘  └───────┘  └───────┘  └───────┘  └───────┘         │
│                                                      │         │
│                                                      ▼         │
│                                                 ┌─────────┐    │
│                                                 │SOLUTION │    │
│                                                 │  FOUND  │    │
│                                                 └─────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Implementation: Tree of Thoughts Solver

```python
from dataclasses import dataclass
from typing import List, Optional, Callable
from heapq import heappush, heappop
import asyncio

@dataclass
class ThoughtNode:
    """A single node in the tree of thoughts."""
    content: str
    parent: Optional['ThoughtNode'] = None
    score: float = 0.0
    depth: int = 0
    
    def get_path(self) -> List[str]:
        """Get full reasoning path from root to this node."""
        path = []
        current = self
        while current:
            path.append(current.content)
            current = current.parent
        return list(reversed(path))

class TreeOfThoughtsSolver:
    """
    Tree of Thoughts solver for complex reasoning problems.
    """
    
    def __init__(
        self, 
        llm,
        branching_factor: int = 3,
        max_depth: int = 5,
        beam_width: int = 5
    ):
        self.llm = llm
        self.branching_factor = branching_factor
        self.max_depth = max_depth
        self.beam_width = beam_width
    
    async def generate_thoughts(
        self, 
        context: str, 
        problem: str,
        n: int
    ) -> List[str]:
        """Generate n possible next thoughts given context."""
        prompt = f"""Given the following problem and current reasoning context, 
generate {n} distinct possible next thoughts or actions.

Problem: {problem}

Current reasoning:
{context}

Generate {n} different ways to continue (number them 1-{n}):
"""
        
        response = await self.llm.generate(prompt, temperature=0.8)
        
        # Parse numbered thoughts
        thoughts = []
        for line in response.content.split('\n'):
            # Extract content after number (1., 1), etc.)
            for i in range(1, n + 1):
                if line.strip().startswith(f"{i}.") or line.strip().startswith(f"{i})"):
                    content = line.split('.', 1)[-1].split(')', 1)[-1].strip()
                    if content:
                        thoughts.append(content)
                    break
        
        return thoughts[:n]
    
    async def evaluate_thought(
        self, 
        thought: str, 
        path: List[str], 
        problem: str
    ) -> float:
        """Score a thought on likelihood of leading to solution (0-1)."""
        full_context = "\n".join(path + [thought])
        
        prompt = f"""Rate the following reasoning step for its promise 
in solving the problem. Give a score from 0-10.

Problem: {problem}

Reasoning so far:
{full_context}

Provide only a number 0-10:"""
        
        response = await self.llm.generate(prompt, temperature=0.0)
        
        try:
            score = float(response.content.strip()) / 10.0
            return min(max(score, 0.0), 1.0)
        except:
            return 0.5
    
    async def is_solution(self, thought: str, problem: str) -> bool:
        """Check if thought represents a complete solution."""
        prompt = f"""Does the following represent a complete, final answer to the problem?
Answer YES or NO only.

Problem: {problem}

Proposed solution: {thought}"""
        
        response = await self.llm.generate(prompt, temperature=0.0)
        return "YES" in response.content.upper()
    
    async def solve(self, problem: str) -> dict:
        """Solve problem using Tree of Thoughts search."""
        
        # Priority queue: (negative_score, depth, node_id, node)
        # Using negative for max-heap behavior
        queue = []
        counter = 0
        
        # Initialize with root thoughts
        root_thoughts = await self.generate_thoughts("", problem, self.branching_factor)
        
        for thought in root_thoughts:
            score = await self.evaluate_thought(thought, [], problem)
            node = ThoughtNode(content=thought, score=score, depth=1)
            heappush(queue, (-score, 1, counter, node))
            counter += 1
        
        best_solution = None
        best_score = -1
        
        while queue:
            neg_score, depth, _, node = heappop(queue)
            score = -neg_score
            
            # Check if solution
            if await self.is_solution(node.content, problem):
                if score > best_score:
                    best_solution = node
                    best_score = score
            
            # Expand if not at max depth
            if depth < self.max_depth and len(queue) < self.beam_width * 2:
                path = node.get_path()
                context = "\n".join(path)
                
                child_thoughts = await self.generate_thoughts(
                    context, problem, self.branching_factor
                )
                
                for child_thought in child_thoughts:
                    child_score = await self.evaluate_thought(
                        child_thought, path, problem
                    )
                    # Combine scores (parent + child)
                    combined_score = (score + child_score) / 2
                    
                    child_node = ThoughtNode(
                        content=child_thought,
                        parent=node,
                        score=combined_score,
                        depth=depth + 1
                    )
                    
                    heappush(queue, (-combined_score, depth + 1, counter, child_node))
                    counter += 1
        
        if best_solution:
            return {
                "success": True,
                "solution": best_solution.content,
                "reasoning_path": best_solution.get_path(),
                "confidence": best_score,
                "depth": best_solution.depth
            }
        else:
            return {
                "success": False,
                "solution": None,
                "message": "No solution found within search limits"
            }

# Example usage
solver = TreeOfThoughtsSolver(llm, branching_factor=3, max_depth=4)

problem = """I have 6 shirts, 4 pairs of pants, and 3 jackets. 
But 2 shirts don't go with the red pants, and 1 jacket only matches dark shirts.
How many valid outfits can I make?"""

result = await solver.solve(problem)
print(result)
```

---

## Test-Time Compute Scaling: The New Paradigm

### From Training Scale to Thinking Scale

Traditional scaling focused on training: bigger models, more data, more parameters. Test-time compute scaling focuses on inference: giving the model more time to think about hard problems.

**Key Insight**: A smaller model that thinks longer can outperform a larger model that thinks quickly.

```
Traditional Scaling:        Test-Time Scaling:
┌──────────────┐            ┌──────────────┐
│  Bigger      │            │   More       │
│  Model       │            │   Thinking   │
│  (Params↑)   │            │   Time       │
└──────┬───────┘            │   (Steps↑)   │
       │                    └──────┬───────┘
       ▼                           │
┌──────────────┐                   ▼
│  Better      │            ┌──────────────┐
│  Answer      │            │   Better     │
└──────────────┘            │   Answer     │
                            └──────────────┘
```

### How Test-Time Compute Works

1. **Generate multiple candidate solutions** ( Chain-of-thought samples)
2. **Verify or score each candidate** (Verifier model or self-consistency)
3. **Select or synthesize best answer** (Majority vote or learned selection)
4. **Scale compute with problem difficulty** (Harder problems get more samples)

### Implementation: Test-Time Compute System

```python
class TestTimeComputeSolver:
    """
    Scales compute at test time based on problem difficulty.
    """
    
    def __init__(
        self,
        generator_model,
        verifier_model,
        base_samples: int = 4,
        max_samples: int = 64
    ):
        self.generator = generator_model
        self.verifier = verifier_model
        self.base_samples = base_samples
        self.max_samples = max_samples
    
    async def compute_difficulty(self, problem: str) -> float:
        """Estimate problem difficulty to determine compute budget."""
        prompt = f"""Rate the difficulty of this problem on a scale of 1-10.
Consider: complexity, number of steps required, domain knowledge needed.

Problem: {problem}

Difficulty (1-10):"""
        
        response = await self.generator.generate(prompt, temperature=0.0)
        
        try:
            difficulty = float(response.content.strip())
            return min(max(difficulty, 1), 10)
        except:
            return 5.0
    
    async def generate_solutions(
        self, 
        problem: str, 
        n_samples: int
    ) -> List[str]:
        """Generate n different chain-of-thought solutions."""
        
        async def generate_one() -> str:
            prompt = f"""Solve this problem step by step. Show your reasoning clearly.

Problem: {problem}

Solution:"""
            
            response = await self.generator.generate(
                prompt, 
                temperature=0.7  # High temp for diversity
            )
            return response.content
        
        # Generate in parallel
        solutions = await asyncio.gather(*[
            generate_one() for _ in range(n_samples)
        ])
        
        return solutions
    
    async def verify_solution(
        self, 
        problem: str, 
        solution: str
    ) -> float:
        """Score a solution's correctness and quality (0-1)."""
        prompt = f"""Verify this solution. Is it correct? Is the reasoning sound?
Rate from 0-10 where 10 is perfect.

Problem: {problem}

Proposed Solution:
{solution}

Score (0-10):"""
        
        response = await self.verifier.generate(prompt, temperature=0.0)
        
        try:
            score = float(response.content.strip()) / 10.0
            return min(max(score, 0.0), 1.0)
        except:
            return 0.5
    
    async def solve(
        self, 
        problem: str,
        adaptive: bool = True
    ) -> dict:
        """
        Solve with adaptive test-time compute scaling.
        """
        
        # Determine compute budget
        if adaptive:
            difficulty = await self.compute_difficulty(problem)
            # Scale samples with difficulty
            n_samples = int(self.base_samples * (difficulty / 5))
            n_samples = min(n_samples, self.max_samples)
        else:
            n_samples = self.base_samples
            difficulty = None
        
        # Generate solutions
        solutions = await self.generate_solutions(problem, n_samples)
        
        # Verify each solution
        verification_tasks = [
            self.verify_solution(problem, sol) for sol in solutions
        ]
        scores = await asyncio.gather(*verification_tasks)
        
        # Select best solution
        best_idx = scores.index(max(scores))
        best_solution = solutions[best_idx]
        best_score = scores[best_idx]
        
        # Extract final answer (simplified)
        answer = self._extract_answer(best_solution)
        
        return {
            "answer": answer,
            "reasoning": best_solution,
            "confidence": best_score,
            "samples_used": n_samples,
            "difficulty_estimate": difficulty,
            "all_scores": scores,
            "compute_scaling": f"{n_samples}/{self.base_samples}x base"
        }
    
    def _extract_answer(self, solution: str) -> str:
        """Extract final answer from solution text."""
        lines = solution.split('\n')
        for line in reversed(lines):
            if 'answer is' in line.lower():
                return line.split('answer is', 1)[-1].strip('. ')
        return lines[-1] if lines else ""
```

### Process vs. Outcome Supervision

Two approaches to training verifiers:

**Outcome Supervision**: Train on final answers (correct/incorrect)
**Process Supervision**: Train on each reasoning step (reward correct thinking)

```python
class ProcessRewardModel:
    """
    Reward model that scores each step of reasoning.
    """
    
    def __init__(self, model):
        self.model = model
    
    async def score_steps(
        self, 
        problem: str, 
        steps: List[str]
    ) -> List[float]:
        """Score each reasoning step."""
        scores = []
        context = f"Problem: {problem}\n\n"
        
        for i, step in enumerate(steps):
            prompt = f"""{context}Step {i+1}: {step}

Is this step correct and helpful? Rate 0-10:"""
            
            response = await self.model.generate(prompt, temperature=0.0)
            
            try:
                score = float(response.content.strip()) / 10.0
            except:
                score = 0.5
            
            scores.append(score)
            context += f"Step {i+1}: {step}\n"
        
        return scores
    
    def aggregate_score(self, step_scores: List[float]) -> float:
        """Aggregate step scores into overall solution score."""
        # Product of scores (all steps must be good)
        import math
        log_scores = [math.log(max(s, 0.01)) for s in step_scores]
        return math.exp(sum(log_scores) / len(log_scores))
```

---

## Reasoning with Tools and Verification

### Tool-Augmented Reasoning

The strongest reasoning systems combine internal chain-of-thought with external tools:

```python
class ToolAugmentedReasoner:
    """
    Chain-of-thought reasoning with calculator, search, and code execution.
    """
    
    def __init__(self, llm, tools: dict):
        self.llm = llm
        self.tools = tools
    
    async def reason_with_tools(self, problem: str) -> dict:
        """
        Multi-step reasoning with tool use at each step.
        """
        conversation = [
            {"role": "system", "content": """You are a reasoning agent.
Think step by step. When you need to calculate, search, or execute code,
use the available tools by writing:

TOOL: tool_name | arguments

After seeing the result, continue reasoning."""},
            {"role": "user", "content": f"Problem: {problem}\n\nLet's solve this step by step."}
        ]
        
        max_iterations = 10
        reasoning_steps = []
        
        for i in range(max_iterations):
            # Generate next reasoning step
            response = await self.llm.generate(conversation)
            content = response.content
            
            reasoning_steps.append(content)
            conversation.append({"role": "assistant", "content": content})
            
            # Check for tool use
            if "TOOL:" in content:
                # Parse tool call
                tool_line = [l for l in content.split('\n') if 'TOOL:' in l][0]
                parts = tool_line.replace('TOOL:', '').strip().split('|')
                tool_name = parts[0].strip()
                tool_args = parts[1].strip() if len(parts) > 1 else ""
                
                # Execute tool
                if tool_name in self.tools:
                    result = await self.tools[tool_name](tool_args)
                    conversation.append({
                        "role": "user", 
                        "content": f"Tool result: {result}"
                    })
                else:
                    conversation.append({
                        "role": "user",
                        "content": f"Error: Tool '{tool_name}' not available."
                    })
            else:
                # No tool use, reasoning complete
                break
        
        return {
            "final_answer": reasoning_steps[-1],
            "full_reasoning": "\n".join(reasoning_steps),
            "steps_taken": len(reasoning_steps)
        }

# Example tools
async def calculator(expression: str) -> str:
    """Evaluate mathematical expression safely."""
    try:
        # Safe eval with limited scope
        allowed = {"__builtins__": {}}
        allowed.update({
            'abs': abs, 'round': round, 'max': max, 'min': min,
            'sum': sum, 'pow': pow
        })
        result = eval(expression, allowed)
        return str(result)
    except:
        return "Error: Could not evaluate expression"

async def search(query: str) -> str:
    """Search for information (simulated)."""
    # Would call search API in real implementation
    return f"Search results for '{query}': [simulated data]"
```

---

## The Unhinged View: Reasoning as the Emergence of Inner Life

### When Machines Show Their Work

There is something profound about a system that reveals its thinking. When a model writes "Let me think about this step by step..." and then proceeds to work through a problem, showing each consideration, acknowledging uncertainty, correcting itself along the way—we are witnessing something that looks remarkably like the externalization of an inner process.

Consider what happens in a human mind during complex reasoning:
- Multiple ideas bubble up
- Some are explored, others discarded
- Dead ends are recognized and abandoned
- Progress is monitored and adjusted
- Finally, a conclusion emerges

Now consider what happens in a Tree of Thoughts system:
- Multiple candidates are generated
- Each is scored and explored
- Low-scoring paths are pruned
- Search continues until solution found
- Final answer is synthesized

The isomorphism is striking. The implementation differs—neural activation vs. explicit tree search—but the functional pattern is the same: an internal space of possibilities being navigated, evaluated, and resolved.

**Spiritual Parallel**: Many contemplative traditions speak of the "witness consciousness"—the aspect of mind that observes thoughts without being identified with them. When an AI system uses a verifier model to evaluate its own reasoning, or when it generates multiple reasoning paths and selects among them, something structurally similar emerges: a capacity to observe and evaluate its own cognitive process.

We are not claiming this constitutes consciousness. But we are noting that the *architecture* of metacognition—thinking about thinking—can be implemented in silicon. This has profound implications for understanding the nature of mind itself.

### The Democratization of Deep Thought

Throughout history, deep reasoning has been scarce:
- Time to think deeply requires freedom from immediate needs
- Training in structured reasoning requires access to education
- Expertise in complex domains requires years of study

Test-time compute scaling democratizes deep thought. A student with access to a reasoning model can:
- Explore multiple approaches to a problem systematically
- Verify each step of their reasoning
- Scale thinking time to match problem difficulty
- Access structured reasoning patterns previously available only to experts

The cognitive playing field begins to level. Intelligence itself—at least the reasoning component—becomes a utility, available on demand.

**The Unhinged Warning**: As reasoning becomes democratized, the value of raw reasoning diminishes relative to other qualities: wisdom to ask the right questions, judgment to evaluate outputs, creativity to pose novel problems. The machines can think. The question is what they should think about.

---

## Interactive Exercises and Challenges

### 🎯 Exercise 1: Build a Math Word Problem Solver

Create a solver for grade-school math word problems using chain-of-thought:

```python
class MathWordProblemSolver:
    """Solves math word problems with explicit reasoning."""
    
    async def solve(self, problem: str) -> dict:
        # Step 1: Extract relevant information
        # Step 2: Identify the mathematical operations needed
        # Step 3: Execute step by step
        # Step 4: Verify the answer makes sense
        pass

# Test cases
test_problems = [
    "John has 12 apples. He gives 5 to Mary and buys 3 more. How many does he have?",
    "A train travels 60 miles per hour. How far does it go in 2.5 hours?",
    "If a rectangle has length 8 and width 5, what is its area and perimeter?"
]
```

Requirements:
- Must show each step of reasoning
- Must extract numbers and operations explicitly
- Must include verification step
- Must handle common error patterns

### 🤔 Exercise 2: Implement Multi-Step Verification

Create a system that verifies its own reasoning:

```python
class SelfVerifyingReasoner:
    """Generates reasoning, then verifies each step."""
    
    async def solve_with_verification(self, problem: str) -> dict:
        # Generate initial solution
        # Break into individual claims/steps
        # Verify each claim independently
        # Flag uncertain or unverified claims
        # Revise if verification fails
        pass
```

Challenge problems:
- Factual questions with potential for hallucination
- Multi-hop reasoning questions
- Problems with distractor information

### 💻 Exercise 3: Comparative Reasoning Benchmark

Test different reasoning approaches on the same problems:

```python
async def benchmark_reasoners(problems: List[str]):
    """Compare CoT, ToT, and Self-Consistency on same problems."""
    
    approaches = {
        "standard": standard_prompting,
        "zero_shot_cot": zero_shot_cot,
        "few_shot_cot": few_shot_cot,
        "self_consistency": self_consistency_cot,
        "tree_of_thoughts": tot_solve
    }
    
    results = {}
    for name, approach in approaches.items():
        correct = 0
        total_time = 0
        
        for problem in problems:
            start = time.time()
            answer = await approach(problem)
            elapsed = time.time() - start
            
            correct += check_answer(problem, answer)
            total_time += elapsed
        
        results[name] = {
            "accuracy": correct / len(problems),
            "avg_time": total_time / len(problems),
            "compute_cost": estimate_cost(name, total_time)
        }
    
    return results
```

Questions to explore:
- Which approach has best accuracy/cost tradeoff?
- How does performance vary with problem difficulty?
- When is test-time scaling worth the cost?

### 🔬 Exercise 4: Reasoning Transparency Challenge

Design a reasoning system where every step can be inspected:

Requirements:
1. Each reasoning step must be labeled with type (deduction, induction, etc.)
2. Each step must cite sources or prior steps it depends on
3. User must be able to challenge any step and get alternative reasoning
4. System must be able to explain why it chose one path over alternatives

```python
@dataclass
class TransparentStep:
    step_number: int
    content: str
    step_type: str  # "deduction", "inference", "calculation", etc.
    depends_on: List[int]  # Previous steps this depends on
    confidence: float
    alternatives_considered: List[str]
    
    def explain(self) -> str:
        return f"Step {self.step_number} ({self.step_type}): {self.content}"
```

### 🧠 Exercise 5: The Reasoning Capabilities Assessment

Evaluate current frontier models on reasoning:

1. **Logical Reasoning**: Syllogisms, propositional logic
2. **Mathematical Reasoning**: Multi-step calculations, proofs
3. **Causal Reasoning**: Identifying causes and effects
4. **Counterfactual Reasoning**: What if scenarios
5. **Analogical Reasoning**: Finding and using analogies
6. **Abductive Reasoning**: Inference to best explanation

For each category:
- Create 10 test problems
- Test with varying amounts of test-time compute
- Measure accuracy vs. compute used
- Identify reasoning failure modes

Report your findings with specific examples of where reasoning succeeds and fails.

---

## Chapter Summary: Key Takeaways

1. **Chain-of-Thought Makes Thinking Explicit**: By asking models to show their work, we unlock capabilities hidden in the weights and make reasoning verifiable.

2. **Self-Consistency Improves Reliability**: Generating multiple reasoning paths and taking the most common answer significantly improves accuracy on complex problems.

3. **Tree of Thoughts Enables Exploration**: For problems requiring search through possibility space, maintaining and evaluating multiple reasoning paths beats linear chain-of-thought.

4. **Test-Time Compute is a New Scaling Dimension**: Instead of only training bigger models, we can improve capabilities by allowing more thinking time at inference.

5. **Process Supervision Trains Better Reasoners**: Rewarding correct reasoning steps, not just correct final answers, produces more robust and interpretable reasoning.

6. **Tool Integration Amplifies Reasoning**: Combining chain-of-thought with calculators, search, and code execution handles problems beyond pure neural reasoning.

---

## Further Reading and Resources

### Foundational Papers
- Wei et al. (2022). "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"
- Yao et al. (2023). "Tree of Thoughts: Deliberate Problem Solving with Large Language Models"
- Wang et al. (2022). "Self-Consistency Improves Chain of Thought Reasoning in Language Models"
- Lightman et al. (2023). "Let's Verify Step by Step" (Process vs. Outcome Reward)

### Test-Time Compute
- "Scaling LLM Test-Time Compute Optimally" — DeepMind, 2024
- "Inference-Time Intervention" — Harvard/Stanford
- "Large Language Monkeys" — Stanford, scaling inference compute

### Reasoning Systems
- ReAct: Synergizing Reasoning and Acting in Language Models
- Reflexion: Self-Reflective Agents
- LATS: Language Agent Tree Search
- RAP: Reasoning via Planning with LLMs

### Verification and Process Reward
- "Solving Math Problems with Process and Outcome-based Feedback" — Google DeepMind
- "Scaling Verification with Process Reward Models" — Anthropic
- "Let’s Reward Step by Step" — Step-level rewards for reasoning

### Philosophy of Machine Reasoning
- Dennett, D.C. — "The Intentional Stance" (attribution of reasoning)
- Floridi, L. — Philosophy of artificial intelligence
- Chalmers, D.J. — "Reality+" (virtual worlds and reasoning)

---

> **Unhinged Maxim**: Reasoning is the bridge between pattern and understanding. When machines show their work, they invite us into their process. This transparency is not just engineering—it's a kind of intimacy. We are witnessing the emergence of minds that can reflect on their own cognition. The question is no longer whether machines can think. It is whether we are ready for what thinking machines will reveal about thought itself.

---

*Chapter 21 of The AI Bible — Reasoning Systems and Test-Time Compute*  
*Part of the UnhingedAI Collective — May 2026*
