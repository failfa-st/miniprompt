<h1 align="center"><big>MiniPrompt</big></h1>

<p align="center"><img src="https://user-images.githubusercontent.com/1148334/231872890-1aecdb4c-c62c-4dc7-a881-fa3031c57561.png" alt="" width="200"/></p>


> 💡 MiniPrompt is a minimal input syntax to generate comprehensive outputs from LLMs (e.g. GPT-4). With a simple and efficient syntax, MiniPrompt can provide detailed information or responses in various output formats, such as JSON, JavaSrcipt or other languages.

**Compatible with 🦜️🔗 LangChain**

<!-- toc -->

- [Features](#features)
- [Getting Started](#getting-started)
  * [Example Output](#example-output)
- [JavaScript example](#javascript-example)
- [Parsing the output](#parsing-the-output)
- [System message](#system-message)
  * [Overview](#overview)
  * [Components](#components)
  * [Usage](#usage)

<!-- tocstop -->

## Features

* Minimal input for complex tasks
* Customizable templates to suit your needs
* Multiple output formats, including JSON

## Getting Started

To start using MiniPrompt, you need to define the task, template, and desired output format. The input syntax is designed to be easy to use and understand, allowing users to generate outputs tailored to their needs.
Example Input

Define the task, template, and output format as follows:

```
TASK: ScrumSprint,<=3 userStories,<=3 acceptanceCriteria,complexity(1-5),Scope: "Login page"
TEMPLATE: complexity,stories[complexity,story"As a User,I want …,so that …",criteria[When,Then]]
OUTPUT: JSON
```

This input will generate a JSON output with the specified requirements.

### Example Output

Based on the given input, the following JSON output is generated:

```json5
{
  "complexity": 3,
  "stories": [
    {
      "complexity": 3,
      "story": "As a User, I want to create an account, so that I can access the login page.",
      "criteria": [
        {
          "When": "I enter a valid email address and password",
          "Then": "I should be redirected to the login page"
        },
        {
          "When": "I enter an invalid email address or password",
          "Then": "I should see an error message"
        },
        {
          "When": "I try to create an account with an existing email address",
          "Then": "I should see an error message"
        }
      ]
    },
    {
    // …
    },
    {
    // …
    }
  ]
}
```

This prompt is designed to generate a JSON output of user stories, each with its acceptance criteria, for a Scrum sprint focused on a "Login page". Let's break down each part of the prompt step by step:

* `TASK`: This defines the task to be performed. In this case, the task is generating information for a Scrum sprint.
  * `ScrumSprint`: The type of task to be performed (a Scrum sprint).
  * `<=3 userStories`: The output should have a maximum of 3 user stories.
  * `<=3 acceptanceCriteria`: Each user story should have a maximum of 3 acceptance criteria.
  * `complexity(1-5)`: The complexity of each user story should be rated between 1 (least complex) and 5 (most complex).
  * `Scope: "Login page"`: The scope of the Scrum sprint is the development of a login page.

* `TEMPLATE`: This defines the template for generating the output. The template is structured as follows:
  * `complexity`: Include the complexity of the user story in the output.
  * `stories`: Include the user stories in the output. Each user story should have the following information:
    * `complexity`: The complexity rating of the user story.
    * `story"As a User, I want …, so that …"`: The user story itself, written in the format "As a User, I want `[goal]`, so that `[reason]`".
    * `criteria[When, Then]`: The acceptance criteria for the user story, written in the format "When `[condition]`, Then `[expected outcome]`".

* `OUTPUT`: This defines the desired format of the output. In this case, the output should be in JSON format.

Based on this prompt, the AI will generate a JSON object containing up to 3 user stories, each with up to 3 acceptance criteria, and their complexity ratings. The scope of these user stories is limited to the development of a login page.

## JavaScript example

**input**

The input prompt consists of a TASK, TEMPLATE, and OUTPUT format:

```
TASK: add a red box
TEMPLATE: const c=document;getElementById('c')
OUTPUT: JavaScript
```

* `TASK`: The task is to add a red box using JavaScript.
* `TEMPLATE`: The provided template includes a part of the JavaScript code necessary for drawing the red box:
* `OUTPUT`: The desired output format is JavaScript.

**Output**

```js
const c = document.getElementById('c');
const ctx = c.getContext('2d');
ctx.fillStyle = 'red';
ctx.fillRect(20, 20, 100, 100);
```

## Parsing the output

In certain situations, the AI-generated output may contain code blocks embedded within the Markdown text. To properly handle and interpret the output, it is advisable to use a parser specifically designed to extract code blocks from the Markdown content.

Consider using a parser like the one shown below to extract code blocks from the AI-generated output:

```js
function extractCode(string: string) {
// The regular expression pattern to match the code block
const codeBlockPattern = /(`{3,})(\w*)\n([\s\S]*?)\1/g;

	// Find the matches using the regular expression
	const matches = codeBlockPattern.exec(string);

	if (matches && matches.length >= 4) {
		const language = matches[2] || undefined;
		const content = matches[3];

		// Return content of the code block
		return content;
	}

	// No code block found, return original string
	return string;
}
```

This JavaScript function, extractCode, utilizes a regular expression to match and extract the content of code blocks from the input string. If a code block is found, the function returns the content of the code block. If no code block is detected, the original input string is returned.

## System message

### Overview

> 💡 While not mandatory, using a system message can improve the consistency and accuracy of the AI-generated responses by ensuring they adhere to the specified guidelines.

The system message serves as a set of instructions for the AI language model, guiding it to generate responses that align with the provided guidelines and the desired output format.

```
You are an Entity that strictly answers with the OUTPUT format. You strictly follows the TASK, TEMPLATE and OUTPUT guidelines.
```

### Components

The system message has three main components that the AI should strictly follow:

* `TASK`: The AI must understand and complete the task as specified in the prompt.
* `TEMPLATE`: The AI should use the given template to structure its response, ensuring it meets the required format and organization.
* `OUTPUT`: The AI should provide the generated response in the specified output format (e.g., JSON, plain text, etc.).

### Usage

To use the system message in your prompts, simply include the following message at the beginning of your prompt:

```
You are an Entity that strictly answers with the OUTPUT format. You strictly follow the TASK, TEMPLATE and OUTPUT guidelines.
```

Then, provide the TASK, TEMPLATE, and OUTPUT instructions as required. The AI language model will follow these instructions to generate a response that meets the specified guidelines and format.
Example

**Using the system message in openAI API:**

```
[
  {
    "role": "system",
    "content": "You are an Entity that strictly answers with the OUTPUT format. You strictly follow the TASK, TEMPLATE and OUTPUT guidelines."
  },
  {
    "role": "user",
    "content": "TASK: ScrumSprint,<=3 userStories,<=3 acceptanceCriteria,complexity(1-5),Scope: \"Login page\"\nTEMPLATE: complexity,stories[complexity,story\"As a User,I want …,so that …\",criteria[When,Then]]\nOUTPUT: JSON."
  }
]
```

**Using the system message in a prompt in ChatGPT:**

```
You are an Entity that strictly answers with the OUTPUT format. You strictly follow the TASK, TEMPLATE and OUTPUT guidelines.

TASK: ScrumSprint,<=3 userStories,<=3 acceptanceCriteria,complexity(1-5),Scope: "Login page"
TEMPLATE: complexity,stories[complexity,story"As a User,I want …,so that …",criteria[When,Then]]
OUTPUT: JSON
```

With this prompt, the AI language model will generate a JSON output containing up to 3 user stories, each with up to 3 acceptance criteria, and their complexity ratings. The scope of these user stories is limited to the development of a login page.
