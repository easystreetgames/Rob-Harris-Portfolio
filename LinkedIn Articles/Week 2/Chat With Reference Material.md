# Chat With Reference Material

This system enables dynamic AI conversations enhanced by contextual reference material. The assistant accesses relevant documentation based on the current conversation needs rather than loading the entire reference corpus upfront.

## Implementation Details

For each prompt submitted by the user, the system:

1. Converts the prompt into an embedding vector and searches for similar reference document embeddings.  
2. Selects and retrieves the most relevant documentation matches.  
3. Augments the original prompt with the matched reference content.  
4. Sends the combined context to the AI model.

This selective approach prevents context overload while ensuring the AI has necessary information to handle specific requests.

## Key Application

The system was developed to help the AI generate code in a specific scripting language without requiring the full language documentation in every interaction. Instead, only pertinent language reference sections are included based on the user's current request.

## Functionality

The AI maintains conversation continuity while incorporating newly provided reference material, allowing for progressive knowledge accumulation throughout the chat session. This creates a more focused and efficient interaction model compared to including all possible reference material at once.

## Custom Script Excerpt

A custom scripting layer is used to configure a specific prompting flow for each experiment.  Here is the script for the *Chat With Reference Material* prompt logic.

This script configures the chat API, memory stream,and response parser.  A system prompt is prepared to be sent with the first prompt.  Most of the logic to prepare each prompt with associated reference material is in the chatApiRecall component.
```
\_(name=PromptLogic  
    // LLM chat API (contains logic to look up prompt-based memories)  
    chatApiRecall(api=Gemini)

    // create memory stream for MemBot chat agent  
    memoryStream(id=MemBot)

    // add initial system prompt  
    systemDoc(id="system-prompt-recall")

    // LLM response logic parses ESG (Easy Street Games) scripts  
    parseResponse(parser=esg)  
)  
```
