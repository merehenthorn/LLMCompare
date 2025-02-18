# LLMCompare
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LLM Comparison: ChatGPT vs Perplexity</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .container {
            display: flex;
            gap: 20px;
        }
        .column {
            flex: 1;
        }
        textarea {
            width: 100%;
            height: 100px;
        }
        #output {
            display: flex;
            gap: 20px;
        }
        .output-column {
            flex: 1;
            border: 1px solid #ccc;
            padding: 10px;
            min-height: 200px;
        }
    </style>
</head>
<body>
    <h1>LLM Comparison: ChatGPT vs Perplexity</h1>
    <div class="container">
        <div class="column">
            <h2>Input</h2>
            <textarea id="userInput" placeholder="Enter your prompt here..."></textarea>
        </div>
        <div class="column">
            <h2>API Keys</h2>
            <input type="text" id="openaiKey" placeholder="OpenAI API Key">
            <input type="text" id="perplexityKey" placeholder="Perplexity API Key">
        </div>
    </div>
    <button onclick="generateResponses()">Generate Responses</button>
    <div id="output">
        <div class="output-column">
            <h3>ChatGPT Output</h3>
            <div id="chatgptOutput"></div>
        </div>
        <div class="output-column">
            <h3>Perplexity Output</h3>
            <div id="perplexityOutput"></div>
        </div>
    </div>

    <script>
        async function generateResponses() {
            const userInput = document.getElementById('userInput').value;
            const openaiKey = document.getElementById('openaiKey').value;
            const perplexityKey = document.getElementById('perplexityKey').value;

            if (!userInput || !openaiKey || !perplexityKey) {
                alert('Please enter a prompt and both API keys.');
                return;
            }

            // Generate ChatGPT response
            try {
                const chatgptResponse = await fetchChatGPTResponse(userInput, openaiKey);
                document.getElementById('chatgptOutput').innerText = chatgptResponse;
            } catch (error) {
                document.getElementById('chatgptOutput').innerText = 'Error: ' + error.message;
            }

            // Generate Perplexity response
            try {
                const perplexityResponse = await fetchPerplexityResponse(userInput, perplexityKey);
                document.getElementById('perplexityOutput').innerText = perplexityResponse;
            } catch (error) {
                document.getElementById('perplexityOutput').innerText = 'Error: ' + error.message;
            }
        }

        async function fetchChatGPTResponse(prompt, apiKey) {
            const response = await fetch('https://api.openai.com/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    model: 'gpt-3.5-turbo',
                    messages: [{ role: 'user', content: prompt }],
                    max_tokens: 150
                })
            });

            if (!response.ok) {
                throw new Error('ChatGPT API request failed');
            }

            const data = await response.json();
            return data.choices[0].message.content;
        }

        async function fetchPerplexityResponse(prompt, apiKey) {
            const response = await fetch('https://api.perplexity.ai/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    model: 'sonar-medium-chat',
                    messages: [{ role: 'user', content: prompt }],
                    max_tokens: 150
                })
            });

            if (!response.ok) {
                throw new Error('Perplexity API request failed');
            }

            const data = await response.json();
            return data.choices[0].message.content;
        }
    </script>
</body>
</html>
