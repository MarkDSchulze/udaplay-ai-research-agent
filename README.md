# UdaPlay AI Research Agent

UdaPlay is an AI research agent for the video game industry. It combines a local Retrieval-Augmented Generation (RAG) pipeline with web search fallback to answer questions about games, publishers, release dates, platforms, genres, and related industry information.

The project demonstrates:

- Semantic search over a local video game dataset
- Retrieval quality evaluation with an LLM
- Web search fallback using Tavily
- Stateful agent execution
- State-machine-based tool orchestration
- Clear answers with source information

## Getting Started

The project is organized into two main notebooks:

- `Udaplay_01_solution_project.ipynb` — builds and tests the local RAG pipeline
- `Udaplay_02_solution_project.ipynb` — implements and demonstrates the UdaPlay agent

The provided video game JSON files are stored in the `games` directory.

### Dependencies

```text
chromadb>=1.0.4
openai>=1.73.0
pydantic>=2.11.3
python-dotenv>=1.1.0
tavily-python>=0.5.4
````

### Installation

1. Install the required Python dependencies.

2. Create a `config.env` file in the project directory with the required API keys:

```text
OPENAI_API_KEY="YOUR_OPENAI_KEY"
CHROMA_OPENAI_API_KEY="YOUR_OPENAI_KEY"
TAVILY_API_KEY="YOUR_TAVILY_KEY"
OPENAI_BASE_URL="https://openai.vocareum.com/v1"
```

3. Load the environment variables with `python-dotenv`:

```python
from dotenv import load_dotenv

load_dotenv("config.env")
```

4. Run `Udaplay_01_solution_project.ipynb` first to create and populate the ChromaDB vector database.

5. Run `Udaplay_02_solution_project.ipynb` to initialize and test the UdaPlay agent.

> API keys are not included in the repository.

## Testing

The notebooks contain example queries that demonstrate both local retrieval and web-search fallback behavior.

Example queries:

```text
When were Pokémon Gold and Silver released?

Which one was the first 3D platformer Mario game?

Was Mortal Kombat X released for PlayStation 5?
```

### Break Down Tests

The example queries demonstrate different agent behaviors:

```text
Pokémon Gold and Silver
→ Relevant information is found in the local vector database.

Super Mario 64
→ Semantic retrieval identifies the appropriate game record.

Mortal Kombat X / PlayStation 5
→ Local retrieval is insufficient.
→ The retrieval evaluator rejects the local results.
→ The agent falls back to Tavily web search.
```

The intended workflow is:

```text
User Question
    ↓
retrieve_game
    ↓
evaluate_retrieval
    ↓
Useful? ── Yes → Generate answer from local data
    │
    No
    ↓
game_web_search
    ↓
Generate answer using web results
```

## Project Instructions

### Part 1 — Offline RAG Pipeline

`Udaplay_01_solution_project.ipynb`:

* Loads the provided video game JSON files
* Formats each game as a searchable document
* Creates a persistent ChromaDB database
* Uses OpenAI embeddings for semantic retrieval
* Stores game metadata in the vector collection
* Demonstrates semantic search against the game dataset

### Part 2 — Agent Development

`Udaplay_02_solution_project.ipynb` implements the UdaPlay research agent.

The agent includes three primary tools:

#### `retrieve_game`

Performs semantic search against the local ChromaDB game collection.

#### `evaluate_retrieval`

Uses an LLM as a judge to determine whether the retrieved documents contain enough information to answer the user's question.

#### `game_web_search`

Uses the Tavily API to search the web when the local dataset does not provide sufficient information.

The agent maintains conversation state and uses a state-machine workflow to coordinate LLM calls and tool execution.

## Built With

* [ChromaDB](https://www.trychroma.com/) — Vector database and semantic retrieval
* [OpenAI](https://openai.com/) — Language models and embeddings
* [Tavily](https://tavily.com/) — Web search API
* [Pydantic](https://docs.pydantic.dev/) — Structured model outputs and validation
* [python-dotenv](https://pypi.org/project/python-dotenv/) — Environment variable management
* Python — Agent, retrieval, and workflow implementation

## License

See the project license for details.
