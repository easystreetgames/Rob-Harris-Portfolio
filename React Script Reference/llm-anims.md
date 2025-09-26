# LLM Anims

LLM anims provide AI-powered functionality for prompts, embeddings, validation, and RAG (Retrieval Augmented Generation) operations.

## prompt / llm

Executes LLM prompts and handles responses. Supports multiple AI providers (Anthropic, Gemini, Ollama) with conversation management, model selection, and response handling through global variables.

### Parameters for prompt

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- prompt / p / text (string): the prompt text to send to the LLM
- response / res (string): variable name to store the response (default: 'response')
- model (string): specific model name to use (overrides default)
- clearMessages / clear / clearMsgs (boolean): if true, clears conversation history before this prompt
- providerType / provider / llm (string): AI provider to use ('anthropic', 'gemini', 'ollama')
- port / p (number): port number for local providers like Ollama

### Example: prompt

```script
// Basic LLM prompt
Define(prompt1='Write a haiku about coding')
Text(
  llm(p=$prompt1 make='showResponse')
)

Def(id=showResponse
  Text(text=$response y=100)
)
```

```script
// Clear conversation and use specific model
Text(
  llm(clear p='Explain quantum computing' model='llama3.1:8b')
)
```

```script
// Chained prompts with state management
Define(
  prompt1='Write a joke'
  prompt2='Explain your last joke'
)

_(id=conversation
  state(id=step1
    llm(p=$prompt1 make='showResponse')
    update(in=responseReady state=step2)
  )
  state(id=step2
    llm(p=$prompt2 make='showResponse')
  )
)
```

```script
// Prompt with response filtering
Text(
  llm(p='Create a JSON task list' block)
  filter(keyIn='response' filter='json' keyOut='jsonData')
  update(text=$jsonData)
)
```

## rag

Performs Retrieval Augmented Generation by finding relevant memories and using them to enhance LLM prompts. Combines memory search with prompt execution for context-aware AI responses.

### Parameters for rag

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- prompt / p / text (string): the prompt text to send to the LLM with memory context

### Example: rag

```script
// Build knowledge base then use RAG
Text(
  memory(clear)
  memory('[Rob/person] is a [game developer]')
  memory('[Rob] works for [Yahoo]')
  memory('[Rob] is writing a [Solitaire game]')
  memory('[Yahoo] is a [company]')
  rag(p='What does Rob do for work?')
  update(text=$response)
)
```

```script
// RAG with conversation flow
_(
  // First populate memory
  memory('[Paris/city] is the [capital] of [France]')
  memory('[France/country] is in [Europe]')
  memory('[Eiffel Tower] is in [Paris]')

  // Then use RAG for informed responses
  rag(p='Tell me about Paris landmarks')
  update(text=$response y=100)
)
```

## embedding / memory

Manages memory graphs and vector embeddings for AI knowledge storage. Creates entities, relationships, and observations that can be searched and retrieved for context-aware AI interactions.

### Parameters for embedding

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- text / txt (string): text content to process for memory storage
- find (string): search term to find related memories
- related (string): find memories related to a specific entity
- clear / clr (boolean): if true, clears all stored memories
- threshold (float): similarity threshold for search results (0-1, default: 0.5)
- limit (int): maximum number of results to return (default: 5)
- subject (string): subject entity for creating relationships
- subjectType (string): type of the subject entity
- target (string): target entity for creating relationships
- targetType (string): type of the target entity
- relation / relationship (string): type of relationship between entities
- observation (string): observation text to store about entities
- depth (int): depth of relationship traversal in searches

### Example: embedding

```script
// Build a knowledge base
Text(
  memory(clear)
  memory('[Rob/person] is a [game developer]')
  memory('[Rob] works for [Yahoo]')
  memory('[Rob] is writing a [Solitaire game]')
  memory('[Yahoo] is a [company]')
  memory('[Yahoo] has a [game site]')
)
```

```script
// Search for related information
Text(
  memory(find='game developer' threshold=0.3 limit=5)
  update(text=$topFind)
)
```

```script
// Find related entities
Text(
  memory(related='Rob' limit=3)
  update(text='Related to Rob: $relatedEntities')
)
```

```script
// Create structured relationships
Text(
  memory(subject='Alice' subjectType='person'
         target='Google' targetType='company'
         relation='works for'
         observation='Alice is a software engineer at Google')
)
```

## test

AI-powered testing functionality that uses language models to generate, validate, or analyze test cases and results.

### Parameters for test

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: test

```script
// Basic AI testing
Text(
  test()
)
```

## validate

AI validation using language models to check data, code, or content against specified criteria. Can validate JSON schemas, code syntax, or custom validation rules.

### Parameters for validate

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- keyIn / key (string): variable containing data to validate
- validation / type (string): type of validation to perform
- errorMsg / error (string): custom error message for validation failures
- schemaKey / schema (string): variable containing validation schema
- requiredCsv / required (string): CSV data for required field validation
- successAction / success (string): action to take on successful validation
- model (string): specific AI model to use for validation

### Example: validate

```script
// Validate JSON structure
Define(jsonData='{"name":"John","age":30}')
Text(
  validate(key='jsonData' type='json' error='Invalid JSON format')
)
```

```script
// Validate with custom schema
Define(
  userSchema='{"type":"object","required":["name","email"]}'
  userData='{"name":"Alice","email":"alice@example.com","age":25}'
)

Text(
  validate(key='userData' schema='userSchema'
           success='dataValid' error='Schema validation failed')
)
```

```script
// AI-powered code validation
Define(codeSnippet='function test() { return true }')
Text(
  validate(key='codeSnippet' type='script'
           model='gpt-4' error='Code validation failed')
)
```