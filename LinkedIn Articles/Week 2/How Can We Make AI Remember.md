# How Can We Make AI Remember?

Recent developments in AI memory systems are pushing the boundaries of what's possible in human-AI interactions. Stanford's Human-Centered Artificial Intelligence (HAI) research has made significant strides in this area, particularly with their groundbreaking work on memory stream constructs.

## **Stanford's Memory Stream Innovation**

Stanford researchers [released a paper](https://hai.stanford.edu/news/computational-agents-exhibit-believable-humanlike-behavior) in Nov 2023 describing a novel memory stream construct that enables AI agents to store and recall memories. At its core, the system leverages a service that converts sentences into numerical representations called embeddings. These embeddings can be stored and later recalled based on their similarity to other embeddings, allowing an AI agent to access relevant memories during conversations.  The memory stream is reinforced by planning and reflection stages that provide continuity and higher reflection on previous events.

What makes this research particularly fascinating is the emergent behavior observed in their experiments. In one notable instance, an AI agent independently decided to run for office, while another took the initiative to arrange a party. Such spontaneous decision-making represents an exciting development for the gaming industry, particularly in creating more dynamic and realistic non-player characters.

## **Memory Stream Experiments**

Building on Stanford's research, I conducted experiments implementing a local embedding service that I used to study memory streams. This service converts a string of text into a numerical vector (embedding) that is stored in a local database.  The embedding can also be used to look up similar embeddings from the database.

### Storing and Recalling Memories

I started with a basic UI to store and recall memories from a specified memory stream.  Tags and metadata can be attached to each memory to be used during recall.  The similarity threshold can also be adjusted to see how it affects recall.  This experiment helped me understand the abilities and limitations of the embedding service.  It also proved to be handy for preloading memories into a memory stream.

### Chat With Memory

The next experiment is a chat interface that embeds recalled memories into each prompt.  This produces an AI assistant that is more contextually aware and retains historical information about conversations.

Here are more details about the experiment: [Chat With Memory](Chat%20With%20Memory.md)

### Chat With Reference Material

It occurred to me that I could use the metadata associated with each embedding to store a file reference.  This provides a way for relevant reference material to be embedded into each prompt.  This helped me achieve a long-time goal of teaching an AI to use my scripting language to create objects and animations.  The overall language was too much to include in a prompt, but the accumulated knowledge over time allows the AI to produce valid scripts that create scenes in my test environment.

Here are more details about the experiment: [Chat With Reference Material](Agents%20With%20Memory.md)

### Agents With Memory

The fourth experiment is a simulation of multiple agents holding 1 on 1 conversations with each other or the user.  Each agent has a memory stream that is accessed during the conversation to provide historical details.  This allows the agents to form relationships and collaborate on goals.  The conversation is limited to a specific number of turns and then the agents reflect on the conversation and add memories to their streams.

The nature and quality of these interactions proved highly dependent on the system prompts and specific model used for the experiment. Different language models exhibited varying conversational capabilities:

* Llama 3.1:8b (run locally) offered limited responses.  
* Gemini 1.5 Flash (accessed via web API) showed unique conversational patterns.  
* Claude 3.5 Sonnet (accessed via web API) demonstrated more nuanced understanding.

One of the most intriguing findings was the high variability in conversation topics, even when using identical configurations. Multiple runs of the same setup often produced dramatically different conversational trajectories, highlighting the dynamic nature of these AI interactions.

Here are more details about the experiment: [Agents With Memory](Agents%20With%20Memory.md)

## **Looking Forward**

While still experimental, memory streams show promise for creating more dynamic and engaging game worlds. The technology enables NPCs to develop persistent relationships and maintain consistent character traits across interactions.

The emergence of sophisticated memory systems in AI represents not just a technical achievement, but a step toward more natural and meaningful human-AI interactions. As we continue to refine these systems, we may find ourselves closer to creating AI that can truly learn from experience and grow through interaction.

For developers interested in implementation, I recommend starting with simple memory storage and recall systems, then gradually incorporating more complex features like multi-agent interactions and dynamic reference material.
