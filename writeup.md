
# Assignment 3 — Prompting Techniques

## Overview

In this assignment, I experimented with four different prompting techniques to build a simple AI customer support assistant. I used Qwen3 8B running locally through Ollama and tested each technique using the provided Python scripts.

All four exercises passed their tests.

## 1. K-Shot Prompting

### Final Prompt

```text
You are a customer support ticket classifier.

Classify customer messages into exactly one of these categories:
billing, shipping, returns, technical, other.

Examples:

Message: My payment was declined.
Category: billing

Message: My package has not arrived.
Category: shipping

Message: I want to return my shoes.
Category: returns

Message: The application keeps crashing.
Category: technical

Message: I have a general question.
Category: other

Respond with ONLY the lowercase category.
Do not include explanations or punctuation.
/no_think
```

### Sample Input and Output

**Input:** My card was charged twice for order 4412.

**Output:** billing

### Test Results

All 4 tests passed (4/4 correct).

### What I Learned

I learned that giving the AI examples of what I want makes it easier for the model to understand the task. Instead of just telling it to classify messages, I showed it examples of each category. The model was able to classify all four test messages correctly and return only the category name without any unnecessary explanation.

## 2. Chain-of-Thought Prompting

### Final Prompt

```text
You are a customer refund assistant.

Determine whether a refund is allowed using
the provided refund policy.

Evaluate these conditions:
1. Was the item purchased within 30 days?
2. Is the item unused and in original condition?
3. Is the item NOT marked final sale?

A refund is approved only when every
condition is satisfied.

Briefly explain which conditions pass or fail.

End your response with exactly one line:
DECISION: YES
or
DECISION: NO
```

### Sample Input and Output

**Input:** Bought 10 days ago, unused, not final sale.

**Output:** YES

### Test Results

All 3 tests passed (3/3 correct).

### What I Learned

I learned that breaking a problem down into smaller steps helps the AI make decisions based on specific rules. In this exercise, the model had to check three conditions before approving a refund. It correctly rejected purchases that were too old or marked as final sale. I also learned that specifying the output format is important when another program needs to read the AI's answer.

## 3. Tool Calling

### Final Prompt

```text
You are a customer support assistant.

When a customer asks about an order:
1. Find the order ID in their message.
2. Call the get_order_status tool with that ID.
3. Use the tool result to answer.
4. Never guess order information.
```

I also provided a description for the `get_order_status` tool, explaining that it retrieves the current status of a customer order using its order ID.

### Sample Input and Output

**Input:** A customer asks for the status of order 4412.

**Tool called:** get_order_status

**Argument:** order_id = "4412"

**Tool result:** shipped, arriving Friday

### Test Results

The model successfully called the correct tool with order ID 4412.

### What I Learned

I learned that AI models can do more than just generate text. They can also call functions to get information from other sources. In this example, instead of making up an order status, the model identified the order number and called the provided Python function. This is useful because a real customer support assistant could use tools to access order databases or other services.

## 4. Retrieval-Augmented Generation (RAG)

### Final Prompt

```text
You are a customer support assistant.

Answer questions using ONLY the provided Context.

If the answer is not in the Context, say:
"I don't know based on our help center."

Never invent facts.
Keep your response short and helpful.
/no_think
```

I also implemented a simple keyword-based retriever that searches the help-center text and returns relevant sections to the model.

### Sample Input and Output

**Input:** Do you ship internationally?

**Output:** yes, we offer international shipping to most countries.

### Test Results

All 3 tests passed (3/3 correct).

The model also correctly answered the return-policy question and refused to provide the CEO's home address because that information was not in the help-center document.

### What I Learned

I learned that RAG allows an AI to answer questions using information from external documents. Instead of expecting the model to know everything, we first retrieve relevant information and include it in the prompt. I also noticed how important it is to tell the model not to make up information. The simple keyword retriever worked for these tests, although a more advanced retriever would probably be better for larger documents or more complicated questions.

## Final Reflection

This assignment helped me understand how different prompting techniques can make AI models more useful for real applications.

K-shot prompting was useful for teaching the model how to classify messages. Chain-of-thought prompting helped it follow rules when making decisions. Tool calling allowed it to interact with a Python function, while RAG helped it answer questions using information from a document.

The most interesting technique for me was tool calling because it showed how an AI assistant can actually interact with a system instead of just answering questions. I can see how this could be used in real applications.

All of my tests passed, but I also learned that getting correct results on a few examples does not guarantee that an AI will always behave correctly. Clear instructions, testing, and access to reliable information are all important when building AI-powered applications.
