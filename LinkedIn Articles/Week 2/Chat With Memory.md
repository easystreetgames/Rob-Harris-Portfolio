# Chat With Memory

This experiment is a chat system that enhances AI assistant interactions through contextual memory integration.  The result is an assistant that recalls details from previous conversations while storing new memories.

## System Architecture

The system implements a conversational loop that incorporates relevant historical context ("memories") into each interaction. The process begins when a user initiates a chat session and enters their first prompt, which may include system-level instructions to configure the AI assistant's behavior or expertise domain.

## Core Workflow

For each user prompt, the system:  
1\. Queries an embeddings service to retrieve semantically similar memories  
2\. Combines the retrieved memories with the current user prompt  
3\. Sends this enriched context to the AI assistant for processing  
4\. Maintains conversation history to preserve chat context

The system continuously repeats this process for each subsequent user interaction, ensuring that each response benefits from both immediate conversation context and relevant historical information.

This memory-enhanced architecture enables more contextually aware and historically informed AI responses while maintaining natural conversation flow.

## Custom Script Excerpt

A custom scripting layer is used to configure a specific prompting flow for each experiment.  Here is the script for the *Chat With Memory* prompt logic.

This script configures the chat API, memory stream,and response parser.  A system prompt and initial message are sent to start the conversation.
```
\_(name=PromptLogic  
    // create LLM interface  
    chatApi(in=sendMsg api=Gemini)

    // create memory stream for MemBot  
    memoryStream(id=MemBot)

    // fetch memories for system prompt  
    memory(recall="\*" max=10 threshold=.2)

    // load and the system prompt  
    systemDoc(id="system-prompt")

    // send the first prompt  
    anim(d=1 out=sendMsg data="r:prompt-01")

    // parse each response (fetches similar memories based on each response)  
    parseResponse(parser=memory)  
)
```
