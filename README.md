# Multi-Model-AI-Playground
Compare responses from multiple free LLM providers in real-time

# AI Playground - File by File

---

## FILE: `package.json`

```json
{
  "name": "ai-playground",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "14.0.4",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "lucide-react": "^0.263.1"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.32",
    "tailwindcss": "^3.3.6"
  }
}
```

---

## FILE: `next.config.js`

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
}

module.exports = nextConfig
```

---

## FILE: `tailwind.config.js`

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

## FILE: `postcss.config.js`

```javascript
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

---

## FILE: `styles/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

---

## FILE: `pages/_app.js`

```javascript
import '@/styles/globals.css'

export default function App({ Component, pageProps }) {
  return 
}
```

---

## FILE: `pages/index.js`

```javascript
import React, { useState } from 'react';
import { Send, Sparkles, Zap, Brain, Code } from 'lucide-react';

export default function AIPlayground() {
  const [input, setInput] = useState('');
  const [responses, setResponses] = useState({});
  const [loading, setLoading] = useState({});
  const [activeTab, setActiveTab] = useState('chat');

  const models = [
    {
      id: 'huggingface',
      name: 'HuggingFace',
      icon: Brain,
      endpoint: '/api/chat-hf',
      color: 'from-yellow-500 to-orange-500'
    },
    {
      id: 'groq',
      name: 'Groq',
      icon: Sparkles,
      endpoint: '/api/chat-groq',
      color: 'from-blue-500 to-cyan-500'
    },
    {
      id: 'together',
      name: 'Together AI',
      icon: Zap,
      endpoint: '/api/chat-together',
      color: 'from-purple-500 to-pink-500'
    }
  ];

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!input.trim()) return;

    const prompt = input;
    setInput('');
    
    models.forEach(async (model) => {
      setLoading(prev => ({ ...prev, [model.id]: true }));
      
      try {
        const response = await fetch(model.endpoint, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ prompt }),
        });

        const data = await response.json();
        
        if (data.error) {
          setResponses(prev => ({ 
            ...prev, 
            [model.id]: `Error: ${data.error}\n\nMake sure to add your API key in environment variables.` 
          }));
        } else {
          setResponses(prev => ({ ...prev, [model.id]: data.text }));
        }
      } catch (error) {
        setResponses(prev => ({ 
          ...prev, 
          [model.id]: `Error: ${error.message}` 
        }));
      } finally {
        setLoading(prev => ({ ...prev, [model.id]: false }));
      }
    });
  };

  const setupGuide = `# Setup Instructions

## 1. Get Free API Keys
- HuggingFace: https://huggingface.co/settings/tokens
- Groq: https://console.groq.com (14,400 requests/day)
- Together AI: https://api.together.xyz/signup

## 2. Create .env.local
HUGGINGFACE_API_KEY=hf_xxxxx
GROQ_API_KEY=gsk_xxxxx
TOGETHER_API_KEY=xxxxx

## 3. Deploy to Vercel
vercel
# Add env vars in dashboard`;

  const codeExample = `// pages/api/chat-groq.js

export default async function handler(req, res) {
  const { prompt } = req.body;

  const response = await fetch(
    'https://api.groq.com/openai/v1/chat/completions',
    {
      method: 'POST',
      headers: {
        'Authorization': \`Bearer \${process.env.GROQ_API_KEY}\`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        model: 'mixtral-8x7b-32768',
        messages: [{ role: 'user', content: prompt }],
      }),
    }
  );

  const data = await response.json();
  return res.json({ text: data.choices[0].message.content });
}`;

  return (
    
      
        
        
          
            
            
              Multi-Model AI Playground
            
          
          
            Compare responses from multiple free LLM providers
          
        

        
          <button
            onClick={() => setActiveTab('chat')}
            className={`px-6 py-3 font-semibold transition-colors ${
              activeTab === 'chat'
                ? 'text-purple-400 border-b-2 border-purple-400'
                : 'text-gray-400 hover:text-gray-300'
            }`}
          >
            Chat Interface
          
          <button
            onClick={() => setActiveTab('setup')}
            className={`px-6 py-3 font-semibold transition-colors ${
              activeTab === 'setup'
                ? 'text-purple-400 border-b-2 border-purple-400'
                : 'text-gray-400 hover:text-gray-300'
            }`}
          >
            Setup Guide
          
          <button
            onClick={() => setActiveTab('code')}
            className={`px-6 py-3 font-semibold transition-colors ${
              activeTab === 'code'
                ? 'text-purple-400 border-b-2 border-purple-400'
                : 'text-gray-400 hover:text-gray-300'
            }`}
          >
            Code Example
          
        

        {activeTab === 'chat' && (
          <>
            
              
                <textarea
                  value={input}
                  onChange={(e) => setInput(e.target.value)}
                  placeholder="Ask anything... (e.g., 'Explain quantum computing')"
                  className="w-full bg-gray-900 text-white rounded-lg p-4 min-h-[120px] resize-none focus:outline-none focus:ring-2 focus:ring-purple-500"
                />
                
                  
                    Send to all models simultaneously
                  
                  
                    
                    Send
                  
                
              
            

            
              {models.map((model) => {
                const Icon = model.icon;
                return (
                  
                    
                      
                        
                        
                          {model.name}
                        
                      
                    
                    
                    
                      {loading[model.id] ? (
                        
                          
                        
                      ) : responses[model.id] ? (
                        
                          {responses[model.id]}
                        
                      ) : (
                        
                          Submit a prompt to see response
                        
                      )}
                    
                  
                );
              })}
            
          </>
        )}

        {activeTab === 'setup' && (
          
            
              {setupGuide}
            
          
        )}

        {activeTab === 'code' && (
          
            
              
              API Route Example
            
            
              {codeExample}
            
          
        )}

        
          Built with React, TailwindCSS, and Free AI APIs
          Deploy to Vercel • 100% Free Tier
        
      
    
  );
}
```

---

## FILE: `pages/api/chat-hf.js`

```javascript
export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { prompt } = req.body;

  if (!prompt) {
    return res.status(400).json({ error: 'Prompt is required' });
  }

  try {
    const response = await fetch(
      'https://api-inference.huggingface.co/models/mistralai/Mistral-7B-Instruct-v0.2',
      {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${process.env.HUGGINGFACE_API_KEY}`,
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          inputs: prompt,
          parameters: {
            max_new_tokens: 500,
            temperature: 0.7,
            return_full_text: false,
          }
        }),
      }
    );

    if (!response.ok) {
      const errorText = await response.text();
      throw new Error(`HuggingFace API error: ${response.status} - ${errorText}`);
    }

    const data = await response.json();
    const text = data[0]?.generated_text || 'No response generated';

    return res.status(200).json({ text });
  } catch (error) {
    console.error('HuggingFace API error:', error);
    return res.status(500).json({ 
      error: error.message || 'Failed to get response from HuggingFace' 
    });
  }
}
```

---

## FILE: `pages/api/chat-groq.js`

```javascript
export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { prompt } = req.body;

  if (!prompt) {
    return res.status(400).json({ error: 'Prompt is required' });
  }

  try {
    const response = await fetch(
      'https://api.groq.com/openai/v1/chat/completions',
      {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${process.env.GROQ_API_KEY}`,
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          model: 'mixtral-8x7b-32768',
          messages: [
            {
              role: 'user',
              content: prompt
            }
          ],
          temperature: 0.7,
          max_tokens: 500,
        }),
      }
    );

    if (!response.ok) {
      const errorText = await response.text();
      throw new Error(`Groq API error: ${response.status} - ${errorText}`);
    }

    const data = await response.json();
    const text = data.choices[0]?.message?.content || 'No response generated';

    return res.status(200).json({ text });
  } catch (error) {
    console.error('Groq API error:', error);
    return res.status(500).json({ 
      error: error.message || 'Failed to get response from Groq' 
    });
  }
}
```

---

## FILE: `pages/api/chat-together.js`

```javascript
export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { prompt } = req.body;

  if (!prompt) {
    return res.status(400).json({ error: 'Prompt is required' });
  }

  try {
    const response = await fetch(
      'https://api.together.xyz/v1/chat/completions',
      {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${process.env.TOGETHER_API_KEY}`,
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          model: 'mistralai/Mixtral-8x7B-Instruct-v0.1',
          messages: [
            {
              role: 'user',
              content: prompt
            }
          ],
          temperature: 0.7,
          max_tokens: 500,
        }),
      }
    );

    if (!response.ok) {
      const errorText = await response.text();
      throw new Error(`Together AI API error: ${response.status} - ${errorText}`);
    }

    const data = await response.json();
    const text = data.choices[0]?.message?.content || 'No response generated';

    return res.status(200).json({ text });
  } catch (error) {
    console.error('Together AI API error:', error);
    return res.status(500).json({ 
      error: error.message || 'Failed to get response from Together AI' 
    });
  }
}
```

---

## FILE: `.env.local.example`

```
HUGGINGFACE_API_KEY=your_key_here
GROQ_API_KEY=your_key_here
TOGETHER_API_KEY=your_key_here
```

---

## FILE: `.gitignore`

```
# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# next.js
/.next/
/out/

# production
/build

# misc
.DS_Store
*.pem

# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# local env files
.env*.local

# vercel
.vercel

# typescript
*.tsbuildinfo
next-env.d.ts
```

---

## FILE: `README.md`

```markdown
# Multi-Model AI Playground

A modern portfolio project showcasing multiple free LLM APIs in a beautiful React interface.

## 🚀 Features

- Compare responses from HuggingFace, Groq, and Together AI
- Beautiful gradient UI with smooth animations
- Serverless architecture with Vercel API routes
- 100% free tier compatible

## 📦 Installation

```bash
npm install
```

## 🔑 Get API Keys

1. **HuggingFace**: https://huggingface.co/settings/tokens
2. **Groq**: https://console.groq.com
3. **Together AI**: https://api.together.xyz/signup

## ⚙️ Configuration

Create `.env.local`:

```
HUGGINGFACE_API_KEY=your_key
GROQ_API_KEY=your_key
TOGETHER_API_KEY=your_key
```

## 🏃 Run Locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## 🚢 Deploy to Vercel

```bash
vercel
```

Add environment variables in Vercel dashboard.

## 📝 License

MIT
```

