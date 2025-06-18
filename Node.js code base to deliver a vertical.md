# Node.js code base to deliver an interactive AI Tutor

Here is a working Node.js codebase for a generative AI application in the education vertical: a **Personalized AI Tutor**.

This command-line application acts as an interactive homework helper. It uses the Google Generative AI SDK to help a student understand a topic without simply giving them the answer. This is accomplished by providing the AI model with a specific "personality" or system instruction[^1].

### Vertical: AI in Education

Generative AI is being used to create personalized learning experiences and intelligent tutoring systems[^2][^3]. This example demonstrates how to build a simple but effective AI tutor that can guide a student through a problem, fostering a better understanding of the subject matter[^4].

---

### Prerequisites

Before you begin, ensure you have the following:

1. **Node.js**: You must have Node.js version 18 or higher installed[^5].
2. **Google AI API Key**: You need a Gemini API key. You can get one for free from Google AI Studio[^5].

---

### Project Setup

1. Create a new folder for your project and navigate into it.
2. Initialize a Node.js project.

```bash
npm init -y
```

3. Install the required packages: the Google Gen AI SDK and `dotenv` for managing your API key securely[^6].

```bash
npm install @google/genai dotenv
```

4. Create a file named `.env` and add your API key to it. This keeps your key out of your source code[^7].

```
API_KEY="YOUR_API_KEY_HERE"
```

5. Create a file named `index.js`. This is where the application logic will reside.

---

### The Codebase: `index.js`

Copy the following code into your `index.js` file. The code is commented to explain each step of the process.

```javascript
// index.js

// Import necessary libraries.
// '@google/genai' is the official Google SDK for the Gemini API.
// 'dotenv' is used to load environment variables from a .env file.
// 'readline' allows us to read input from the terminal.
import { GoogleGenerativeAI, HarmCategory, HarmBlockThreshold } from "@google/genai";
import dotenv from "dotenv";
import * as readline from "node:readline/promises";

// Load environment variables from the .env file.
dotenv.config();

// Initialize the GoogleGenerativeAI client with the API key from environment variables [^2].
const genAI = new GoogleGenerativeAI(process.env.API_KEY);

// Set up the readline interface for interactive command-line input and output [^9].
const terminal = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

async function runTutor() {
  console.log("Welcome to your Personal AI Tutor! Ask me a homework question.");
  console.log("Type 'exit' to end the session.\n");

  // Define the system instruction for the model.
  // This instruction guides the AI to act as a tutor, prompting the student to think
  // critically instead of just providing direct answers [^4].
  const systemInstruction = {
    role: "system",
    parts: [{ text: "You are a friendly and encouraging tutor for high school students. Your goal is to help the student understand concepts, not to give them the answer directly. When a student asks a question, guide them with leading questions and hints. For example, if they ask 'What is the powerhouse of the cell?', you should respond with something like, 'That's a great question! It's a tiny organelle inside the cell. Do you remember what it's called? It starts with the letter M...'"}]
  };
  
  // Get the generative model, applying the system instruction.
  // We use gemini-1.5-flash, a fast and capable model [^2].
  const model = genAI.getGenerativeModel({
    model: "gemini-1.5-flash",
    systemInstruction: systemInstruction,
  });

  // Start a chat session with an empty history. The system instruction is not part of the history.
  const chat = model.startChat({
    history: [],
    // Safety settings can be adjusted to control what content is blocked [^4].
    safetySettings: [
      {
        category: HarmCategory.HARM_CATEGORY_DANGEROUS_CONTENT,
        threshold: HarmBlockThreshold.BLOCK_MEDIUM_AND_ABOVE,
      },
    ],
  });

  // Start the main conversation loop.
  while (true) {
    const userInput = await terminal.question("You: ");

    if (userInput.toLowerCase() === 'exit') {
      console.log("\nGreat session! Keep up the great work.");
      break;
    }

    // Send the user's message to the model and get a streaming response [^9].
    const result = await chat.sendMessageStream(userInput);
    
    process.stdout.write("Tutor: ");
    // Iterate through the stream and print the response chunks to the console in real-time.
    for await (const chunk of result.stream) {
      const chunkText = chunk.text();
      process.stdout.write(chunkText);
    }
    process.stdout.write("\n\n");
  }

  // Close the terminal interface.
  terminal.close();
}

// Run the main function and catch any errors.
runTutor().catch(console.error);

```


### How to Run the Application

1. Save all your files.
2. Open your terminal in the project directory.
3. Run the following command:

```bash
node index.js
```

4. The application will start, and you can begin asking it homework questions.

### Scaling from Prototype to Enterprise

This simple command-line tool demonstrates the core functionality of a generative AI application[^6]. To evolve this into a scalable, enterprise-grade solution, you could:

* **Build a Web Interface**: Wrap the Node.js logic in a web framework like Express.js to create a web application that students can access from any browser[^8].
* **Deploy to the Cloud**: Move from running the application locally to deploying it on a scalable, managed platform like Google Cloud's Vertex AI[^1]. The unified SDK ensures the code you wrote for prototyping can be used in a production environment with minimal changes.
* **Add Multimodal Capabilities**: Enhance the application to accept images. A student could upload a picture of a math problem or a biology diagram, and the AI tutor could analyze it and provide guidance[^7].
* **Integrate with Learning Systems**: Connect the tutor to a school's Learning Management System (LMS) to track student progress and tailor assistance to specific curriculum goals.

<div style="text-align: center">⁂</div>

[^1]: https://cloud.google.com/nodejs/docs/reference/vertexai/latest

[^2]: https://tecknexus.com/top-175-generative-ai-companies-transforming-industries/

[^3]: https://springsapps.com/knowledge/revolutionizing-education-with-openais-realtime-api-ai-teachers-are-getting-close-to-natural-conversations

[^4]: https://www.vertical.vc/generative-ai/

[^5]: https://ai.google.dev/gemini-api/docs/quickstart

[^6]: https://ai-sdk.dev/docs/getting-started/nodejs

[^7]: https://www.npmjs.com/package/@google/generative-ai/v/0.10.0

[^8]: https://www.npmjs.com/package/@google/generative-ai/v/0.8.0

[^9]: https://www.linkedin.com/pulse/generative-ai-top-100-use-cases-different-industry-dr-rabi-prasad-fomyc

[^10]: https://huggingface.co/blog/lynn-mikami/google-gen-ai-sdk

[^11]: https://github.com/IBM/ibm-generative-ai-node-sdk

[^12]: https://github.com/mohitejaikumar/generative-ai-js

[^13]: https://ai.google.dev/gemini-api/docs/image-generation

[^14]: https://cloud.google.com/vertex-ai/generative-ai/docs/sdks/overview

[^15]: https://github.com/googleapis/js-genai

[^16]: https://cloud.google.com/vertex-ai/docs/samples

[^17]: https://googleapis.github.io/js-genai/

[^18]: https://cloud.google.com/vertex-ai

[^19]: https://datasciencedojo.com/blog/nodejs-libraries-machine-learning/

