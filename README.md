# Persistence in LangGraph

A hands-on notebook demonstrating how **LangGraph checkpointers** persist graph state, using a simple two-step LLM workflow (joke → explanation).

## What it covers

| Section | Concept |
|---|---|
| **Checkpointing** | Compile a graph with `InMemorySaver`; every step is saved per `thread_id`. |
| **Threads** | Run independent conversations (`pizza` on thread 1, `pasta` on thread 2) with isolated state. |
| **State inspection** | `get_state()` and `get_state_history()` to view the current and past checkpoints. |
| **Time travel** | Replay execution from an earlier `checkpoint_id`. |
| **Updating state** | `update_state()` to edit a past checkpoint (e.g. change the topic to `samosa`) and re-run from there. |
| **Fault tolerance** | Interrupt a graph mid-run (during a hanging Step 2), then resume with `invoke(None, ...)` from the last saved checkpoint instead of starting over. |

## Setup

```bash
pip install langgraph langchain-mistralai python-dotenv
```

Create a `.env` file with your MistralAI key:

```
MISTRALAI_API_KEY=your_key_here
```

## Usage

1. Open `persistence.ipynb` and run the cells in order.
2. For the **Fault Tolerance** section, run the crash cell, press the notebook's **Stop** button while Step 2 hangs, then run the next cell to resume.
3. Checkpoint IDs in the time-travel cells are specific to the original run; replace them with IDs from your own `get_state_history()` output.

## Notes

- `InMemorySaver` keeps checkpoints in memory only, so they are lost when the kernel restarts. For durable persistence, use a database-backed checkpointer (e.g. SQLite or Postgres).
