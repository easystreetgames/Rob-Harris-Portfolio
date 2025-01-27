# Agents With Memory

## Agent Conversations with Memory Streams

The system enables one-on-one conversations between simulated agents and between agents and the user. A matchmaker initiates conversations between idle agents using a system prompt to define behavior. The process leverages memory retrieval, where relevant memories about conversation partners inform interactions.

## Conversation Flow

When matching two agents, one initiates with a greeting enriched by fetched memories about their conversation partner. Conversations proceed for a fixed number of turns before agents provide summaries, which generate new memories about their interaction partners. User-initiated conversations follow the same pattern, with agents treating the user as another agent.

## Conversation Variability Factors

The system produces diverse conversation outcomes based on prompting parameters (context provided, word/turn limits) and the AI model used (Llama 3.1:8b locally, Sonnet 3.5 and Gemini 1.5 Flash via web APIs). Even identical configurations can produce substantially different conversational patterns and outcomes when run again.

## Custom Script Excerpt

A custom scripting layer is used to configure a specific prompting flow for each experiment.  Here is the script for the Agent Simulation prompt logic.

This script is much more complicated than the [Chat With Memory](https://github.com/easystreetgames/Rob-Harris-Portfolio/blob/main/Chat%20With%20Memory.md) and [Chat With Reference Material](https://github.com/easystreetgames/Rob-Harris-Portfolio/blob/main/Chat%20With%20Reference%20Material.md) scripts.  It uses the same basic components of chat API, memory stream, and response parser.  They are enclosed in a state machine that is controlled by a behavior tree.  This allows the agents to operate with some autonomy for match-made conversations with another agent or the user.  When not in conversation, they can perform planning or reflection activities.

The scripting layer is convenient for fine-tuning the logic flow and experimenting quickly with variations.  It is a good example of combining classing NPC mechanisms with AI prompting.
```
// definition for behavior tree used with each agent’s state machine  
tree(gen\_agent  
    selector(  
        sequence(  
            condition(in\_conversation)  
		// change to conversation state  
            action(talk)  
        )  
        sequence(  
            condition(should\_plan)  
		// change to planning state  
            action(planning)  
        )  
        sequence(  
            condition(should\_reflect)  
		// change to reflection state  
            action(do\_reflect)  
        )  
        action(wait)  
    )  
)

// prototype for agent icon and state machine  
Cube(name=AgentProto proto=true scale=2,2,.5  
    // agent cube with picture and subtitle  
    Cube(name=pic pos=0,0,-.25 scale=.95,.95,.1)  
    Text(text=%id pos=0,-1.25,-.25 scale=1,1,1 size=2)

    // set flag to do initial planning  
    set(should\_plan=true)

    // LLM interface  
    chatApi(api=Gemini)

    // set up agent’s memory stream  
    memoryStream()

    // agent state machine  
    machine(in=agentState  
	  // reference to behavior tree defined above  
        tree(gen\_agent)

	  // agent states  
        wait(  
		// change to idle color  
            colorCurve(target=green dur=1 block)

		// change to idle position  
            changePosition(current=true axis=y e=4.75 dur=1 curve=ease-in-out)  
        )  
        talk(  
		// change to talking color  
            colorCurve(target=cyan dur=1)

		// change to talking position  
            changePosition(current=true axis=y e=5 dur=1 curve=ease-in-out)

            // add conversation logic that passes prompts to other agent  
            talkTo(turns=3 intro="intro-prompt" summary="summary-prompt")

            // fetch memories of other agent  
            memory(recall="\*" tags=%otherName max=5 threshold=.2)

            // system prompt with memories included  
            systemDoc(id="system-prompt-Agent1")

            // response handler for conversations  
            parseResponse(block parser=convo)

		// set flag to reflect on recent memories  
            set(should\_reflect=true)  
        )  
        planning(  
		// change to planning color  
            colorCurve(target=purple dur=2 block)

            // system prompt for planning phase  
            systemDoc(id="planning-Agent1”)

            // send blank message (all the info is in the system prompt)  
            anim(out=sendMsg data=" ")

            // response handler  
            parseResponse(block parser=planning)

		// done planning  
            set(should\_plan=false)  
        )  
        do\_reflect(  
		// change to reflecting color  
            colorCurve(target=red dur=1 block)

            // fetch recent memories about other agent  
            memory(recall="\*" tags=%otherName max=10 threshold=.2)

            // system prompt for reflection phase  
            systemDoc(id="reflect-prompt”)

            // send blank message (all the info is in the system prompt)  
            anim(out=sendMsg data=" ")

            // response handler for reflection phase  
            parseResponse(block parser=reflect)

		// done reflecting  
            set(should\_reflect=false)  
        )  
    )  
)  
```
