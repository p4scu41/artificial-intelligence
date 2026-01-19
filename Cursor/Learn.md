https://cursor.com/learn

AI models They are probabilistic. This means there are many different paths the model might take given the same input. The first piece of your AI mental model is to never assume you are guaranteed to get the same answer every time.

These models are predicting the next chunk of text to respond based on two things:

1. The information the model was “trained” on
2. What you provide the model as an input, called a “prompt”

## Models

AI models vary in their level of intelligence, speed to respond, cost, and areas of expertise. Some models are fast and cheap, but cannot solve deeper technical problems which require more thinking.

Other models are slower and more expensive, but can “think” and work on problems for significantly longer when you have more complicated tasks.

## Modalities

You can also interact with models in different ways, or “modalities”. For example, through text into a chatbot, generating an image, talking to a virtual AI, or even generating video from prompts.

## Hallucinations

Hallucination is when an AI model confidently generates information that seems plausible but is actually incorrect.

When an AI model doesn't know something, it doesn't always say "I don't know." Instead, it generates what seems most likely based on patterns it has seen. For coding, this might mean:

- Inventing plausible-sounding API methods that don't actually exist
- Mixing up syntax between different programming libraries or frameworks
- Creating configuration options that seem reasonable but aren't real

AI model providers take tons of text on the internet (and other proprietary data) to train models up until some date called the “knowledge cutoff”. This date is the latest knowledge models have. AI models may suggest incorrect solutions if you ask about libraries created after this date.

Which is the best immediate response to a likely hallucinated API method?
Verify in docs or your codebase; provide the error back to the model.

Models can also confidently suggest the wrong answer.

The key to working effectively with AI is developing a verification mindset. Every suggestion is a starting point, not a final answer. This might sound like extra work, but it's actually making you a better developer by forcing you to understand what the code does rather than just copying it.

## Tokens & Pricing

AI models don't work directly with words like "hello" or "world" either. Instead, they break everything down into smaller chunks called tokens.

For example, the word "hello" might be one token, but the word "understanding" could be broken into multiple tokens like "under", "stand", and "ing". Sometimes even parts of words, punctuation, or spaces become their own tokens.

So why does this matter? Two reasons:

- Tokens are how models are priced. You pay per token, not per word or character.
- Tokens are how we measure model speed. Faster models have a faster TPS, or tokens per second, which are returned back to the user.

AI models charge based on two types of tokens:

- Input tokens, which include everything you send to the model like your prompt and the previous conversation.
- Output tokens, which include everything the model generates back to you.

Output tokens typically cost 2-4x more than input tokens, because generating new content requires more computational work than just processing what you sent.

You’ll want to be intentional about how much information you include in your initial context, which we’ll talk about next, and how you steer the model to be concise or detailed in its responses.

### Streaming responses

AI models generate tokens one at a time, in sequence. They predict the next token, then use that prediction to help predict the next token after that, and so on. This is why you see responses appear word by word (or rather, token by token).

Responses can then stream back to you. This is great because you don’t have to wait for the entire response to finish, which could take minutes, and you can interrupt the model if it starts to go off track.

AI tools often use techniques to reduce the number of tokens sent to the underlying models. For example, automatically caching parts of your prompt that you use repeatedly, or helping you manage the context that you include with each request.

## Context

You might think to improve the model output you need to write better prompts. And that will certainly help, but it’s missing the bigger point: working with AI models is all about managing the context you provide it.

Working with AI models for coding:

- You can have many inputs, like your current codebase and files, and a prompt to tell the AI model what you want to achieve
- You follow a plan, sometimes human generated or suggested by the model itself, which can then create a todo list and check items off as it completes tasks
- And the end, you get generated code you can apply to your project

### System and user prompts

Your inputs, as well as the model outputs, all become part of the “context”. Think of the context like a long list, where the AI model keeps a working memory for the conversation.

At the start of the list is a system prompt. This is how the tool creator can inject some instructions or style for the model to follow. It’s trying to help nudge the output in a certain direction, including defining specific rules to follow.

Then you have the user message or prompt. This could be any directions you want to give the model. For example, adding a new route to manage user accounts.

This doesn’t have to be just text. Many AI products now support attaching images, where the underlying AI model can read and understand the contents of the image and include that text representation in the context.

### Including context

Tools like Cursor can automatically include other relevant information in the input context based on the state of your codebase. For example, your open files, the output from your terminal, linter errors, and more.

After sending the inputs to the model, it generates and returns some output back. For simple questions, this might just be text. For coding use cases, this could be snippets of code to apply to your codebase. Everything returned from the model is part of the output context.

Manage Context Growth Remember that context accumulates over time:
- Each message (both input and output) adds to the working memory
- Like human conversation, it becomes harder to "remember" earlier parts as conversations grow longer
- Monitor context limits - AI tools often provide feedback on how close you are to the limit
- Consider compressing or summarizing long conversations to stay under limits

Be Mindful of Context Windows Different AI models have different context limits. As conversations grow, you may need to start fresh or summarize to maintain effectiveness.

The core insight: context management is a skill that's just as important as prompt engineering itself!

## Tool Calling

AI models can actually take actions and retrieve information dynamically through tool calling.

Tool calling is like giving those models the ability to call other APIs themselves. It's as if the AI model can learn new skills.

When developers build AI applications, they can define specific "tools" that the AI model can use. These tools are like special abilities that extend what the model can do beyond just thinking and responding with text.

You've probably already used tool calling without realizing it! When you ask ChatGPT to generate an image, search the web, or run code, it's using tools behind the scenes.

Here's what happens under the hood:

1. The AI model receives your request and recognizes it needs additional capabilities
2. It formats a special response in JSON (a structured data format) that specifies which tool to use and what parameters to pass
3. The application runs that tool and returns the results
4. The AI model incorporates those results into its context and continues the conversation

For building software, tools are incredibly powerful because they let the AI model:

- Read and write files in your codebase
- Search through code to find relevant functions or patterns
- Run shell commands to test code or install packages
- Access documentation or search the web for current information
- Check for errors by running linters or tests

Without tools, the AI model would be limited to only the information you explicitly provide in the context. With tools, it can actively explore and interact with your codebase.

Every tool has three main components:

1. A name like read_file or search_web
2. A description that tells the model when and how to use the tool
3. Parameters which are the inputs the tool needs to work

Here's an example of what a tool definition might look like:

```json
{
  "name": "read_file",
  "description": "Read the contents of a file from the codebase",
  "parameters": {
    "filepath": "The path to the file to read"
  }
}
```

When the AI model wants to use this tool, it generates a response like:

```json
{
  "tool": "read_file",
  "parameters": {
    "filepath": "src/components/Button.tsx"
  }
}
```

The application then reads that file and adds the contents to the conversation context, allowing the model to understand your Button component and suggest relevant changes.

Remember how we talked about tokens and pricing? Tool calls consume tokens in two ways:

1. Tool definitions are included in the input context (usually a few hundred tokens per tool)
2. Tool results are added to the output context (varies based on what the tool returns)

This means conversations with lots of tool usage will fill up the context window faster and cost more. But the tradeoff is usually worth it because the AI can be much more helpful with access to real-time information.

Recently, a new standard called ***MCP (Model Context Protocol)*** was created. Think of it as a universal way for AI models to use and integrate tools across applications.

MCP aims to be a standard for connecting tools to AI models. This means developers can build tools once and have them work across many different AI apps.

## Agents

What if we let AI models call multiple tools, make decisions about which tools to use, and even learn from the results? That's where agents come in.

At its core, an agent is simply tools in a loop.

Think of it like this: instead of you having to tell the AI what to do step by step, you give it a goal and let it figure out the steps itself.

### How agents work in practice

Let's say you ask an AI agent to "add a dark mode toggle to my settings page." The agent goes into action, first searching through your codebase to find the settings page. Once it locates the relevant files, it reads them to understand the current structure and styling approach.

Then something interesting happens: the agent creates its own plan for implementing the feature. It might decide to add a state variable, create new CSS classes, implement the toggle component, and update the UI. As it executes each step, it's constantly checking its work, running tests, and fixing any errors that pop up.

This entire process happens through a series of tool calls, with the agent deciding what to do next based on the results of each action. It's like watching someone think out loud, except they're actually doing the work as they go.

The real magic of agents is how they transform your role as a developer. You can simply state your end goal and let the agent handle the journey.

This is large shift because it turns you into a task manager instead of a task doer. You can have multiple agents working on different parts of your codebase simultaneously.

Agents are great at tasks with clear objectives and established patterns, like adding tests to existing code, updating documentation, refactoring with consistent patterns, and fixing bugs with clear error messages.

However, they still struggle with complex debugging that requires deep understanding of system interactions, perfectly matching visual design mockups down to the pixel, working with new libraries that weren't in their training data, and more.

Think of agents like fast junior developers who need clear direction, who also can easily forget things, so they require oversight. They can get stuck in loops, repeating the same failing approach without recognizing they need a different strategy.

Agents use significantly more tokens than asking simple questions because of all the tool calls and iterations. Without good constraints, they might enthusiastically make changes you didn't intend. And crucially, you're still responsible for verifying the code works correctly and meets your standards.

Working effectively with agents is about learning what to delegate and when. Start with smaller, well-defined tasks to build up confidence. As you get comfortable, you can delegate larger chunks of work, but always with checkpoints and verification steps along the way.

The goal isn't to eliminate human involvement but to amplify what you can accomplish. You become the architect and reviewer while agents handle the implementation details.
