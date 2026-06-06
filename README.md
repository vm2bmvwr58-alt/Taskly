# Taskly
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Taskly — Build your own AI</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #111118;
    --border: #1e1e2e;
    --accent: #7c6aff;
    --accent2: #00e5c0;
    --accent3: #ff6a6a;
    --text: #e8e8f0;
    --muted: #6b6b85;
    --link: #9d8fff;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 14px;
    line-height: 1.7;
    min-height: 100vh;
  }

  /* Subtle grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(124,106,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(124,106,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 60px 24px 100px;
    position: relative;
    z-index: 1;
  }

  /* Header */
  header {
    margin-bottom: 56px;
    border-bottom: 1px solid var(--border);
    padding-bottom: 40px;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 28px;
  }

  .logo-icon {
    width: 36px;
    height: 36px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
  }

  .logo-text {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 800;
    letter-spacing: -0.5px;
    color: #fff;
  }

  h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(28px, 5vw, 42px);
    font-weight: 800;
    line-height: 1.15;
    letter-spacing: -1px;
    margin-bottom: 16px;
    color: #fff;
  }

  h1 .highlight {
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .tagline {
    color: var(--muted);
    font-size: 14px;
    max-width: 560px;
    line-height: 1.8;
  }

  .quote {
    margin-top: 24px;
    padding: 16px 20px;
    border-left: 3px solid var(--accent);
    background: rgba(124,106,255,0.06);
    border-radius: 0 6px 6px 0;
    color: var(--muted);
    font-style: italic;
  }

  .quote span {
    color: var(--text);
    font-style: normal;
  }

  /* TOC */
  .toc {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 24px 28px;
    margin-bottom: 56px;
  }

  .toc-title {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 16px;
  }

  .toc-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    gap: 6px 20px;
  }

  .toc-grid a {
    color: var(--link);
    text-decoration: none;
    font-size: 13px;
    transition: color 0.2s;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .toc-grid a::before {
    content: '→';
    color: var(--accent);
    font-size: 11px;
  }

  .toc-grid a:hover { color: var(--accent2); }

  /* Sections */
  .section {
    margin-bottom: 52px;
    animation: fadeUp 0.4s ease both;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 16px;
  }

  .section-tag {
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    padding: 3px 10px;
    border-radius: 20px;
    background: rgba(124,106,255,0.12);
    color: var(--accent);
    border: 1px solid rgba(124,106,255,0.2);
  }

  .section-tag.green {
    background: rgba(0,229,192,0.08);
    color: var(--accent2);
    border-color: rgba(0,229,192,0.2);
  }

  .section-tag.red {
    background: rgba(255,106,106,0.08);
    color: var(--accent3);
    border-color: rgba(255,106,106,0.2);
  }

  h2 {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 700;
    color: #fff;
    letter-spacing: -0.3px;
  }

  .section-line {
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* Links list */
  .links-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .links-list li {
    display: flex;
    align-items: baseline;
    gap: 10px;
    padding: 7px 12px;
    border-radius: 6px;
    transition: background 0.15s;
  }

  .links-list li:hover {
    background: rgba(255,255,255,0.03);
  }

  .lang-badge {
    font-size: 11px;
    font-weight: 500;
    color: var(--accent2);
    min-width: 70px;
    flex-shrink: 0;
  }

  .lang-badge.py { color: #4ec9b0; }
  .lang-badge.js { color: #f0d060; }
  .lang-badge.go { color: #79d4f0; }
  .lang-badge.rs { color: #ff9a6a; }
  .lang-badge.ts { color: #6aaefc; }
  .lang-badge.any { color: var(--muted); }

  .links-list a {
    color: var(--link);
    text-decoration: none;
    transition: color 0.2s;
    font-size: 13.5px;
  }

  .links-list a:hover {
    color: var(--accent2);
    text-decoration: underline;
    text-underline-offset: 3px;
  }

  .badge-new {
    font-size: 9px;
    padding: 2px 7px;
    background: rgba(0,229,192,0.15);
    color: var(--accent2);
    border-radius: 20px;
    border: 1px solid rgba(0,229,192,0.25);
    letter-spacing: 1px;
    text-transform: uppercase;
    font-weight: 600;
    flex-shrink: 0;
  }

  /* Stats bar */
  .stats {
    display: flex;
    gap: 32px;
    padding: 20px 0;
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    margin-bottom: 48px;
  }

  .stat {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .stat-num {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 800;
    color: #fff;
  }

  .stat-label {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  /* Footer */
  footer {
    border-top: 1px solid var(--border);
    padding-top: 32px;
    color: var(--muted);
    font-size: 12px;
    line-height: 1.9;
  }

  footer a {
    color: var(--link);
    text-decoration: none;
  }

  footer a:hover { color: var(--accent2); }

  .contribute {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px 24px;
    margin-bottom: 32px;
  }

  .contribute h3 {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 8px;
  }
</style>
</head>
<body>
<div class="container">

  <header>
    <div class="logo">
      <div class="logo-icon">⚡</div>
      <span class="logo-text">Taskly</span>
    </div>

    <h1>Build your own <span class="highlight">AI</span></h1>

    <p class="tagline">
      Een verzameling van goed geschreven, stap-voor-stap guides om je favoriete AI-technologieën zelf te bouwen — van chatbots tot neurale netwerken, van recommenders tot autonome agents.
    </p>

    <div class="quote">
      "What I cannot create, I do not understand." — <span>Richard Feynman</span>
    </div>
  </header>

  <div class="stats">
    <div class="stat">
      <span class="stat-num">12+</span>
      <span class="stat-label">Categorieën</span>
    </div>
    <div class="stat">
      <span class="stat-num">80+</span>
      <span class="stat-label">Tutorials</span>
    </div>
    <div class="stat">
      <span class="stat-num">100%</span>
      <span class="stat-label">AI-focused</span>
    </div>
  </div>

  <!-- TOC -->
  <div class="toc">
    <div class="toc-title">Inhoud</div>
    <div class="toc-grid">
      <a href="#chatbot">AI Chatbot</a>
      <a href="#llm">Large Language Model</a>
      <a href="#neural">Neuraal Netwerk</a>
      <a href="#recommender">Recommender Systeem</a>
      <a href="#vision">Computer Vision</a>
      <a href="#nlp">NLP & Tekstanalyse</a>
      <a href="#agent">AI Agent</a>
      <a href="#diffusion">Image Generation</a>
      <a href="#rag">RAG & Vector Search</a>
      <a href="#voice">Voice AI</a>
      <a href="#sentiment">Sentiment Analyse</a>
      <a href="#automation">AI Automation</a>
    </div>
  </div>

  <!-- CHATBOT -->
  <div class="section" id="chatbot">
    <div class="section-header">
      <span class="section-tag">Populair</span>
      <h2>Build your own AI Chatbot</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://realpython.com/build-a-chatbot-python-chatterbot/" target="_blank">Build a Chatbot with Python and ChatterBot</a></li>
      <li><span class="lang-badge js">JavaScript</span> <a href="https://dev.to/saurabhmehta1601/build-your-own-ai-chatbot-using-nodejs-and-dialogflow-3k74" target="_blank">AI Chatbot met Node.js en Dialogflow</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://medium.com/@rubentak/talking-to-a-chatbot-using-streamlit-and-openai-d34cae2f0742" target="_blank">Chatbot bouwen met OpenAI API & Streamlit</a></li>
      <li><span class="lang-badge ts">TypeScript</span> <a href="https://vercel.com/templates/next.js/nextjs-ai-chatbot" target="_blank">Full-stack AI Chatbot met Next.js en Vercel AI SDK</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.langchain.com/langchain" target="_blank">Conversational AI bouwen met LangChain</a></li>
      <li><span class="lang-badge js">JavaScript</span> <a href="https://www.youtube.com/watch?v=SYpuf7vKP54" target="_blank">Build a ChatGPT Clone from Scratch [video]</a></li>
    </ul>
  </div>

  <!-- LLM -->
  <div class="section" id="llm">
    <div class="section-header">
      <span class="section-tag green">Geavanceerd</span>
      <h2>Build your own Large Language Model</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/karpathy/llm.c" target="_blank">LLM training in pure C / CUDA — Andrej Karpathy</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/karpathy/nanoGPT" target="_blank">nanoGPT: de eenvoudigste GPT implementatie</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.youtube.com/watch?v=kCc8FmEb1nY" target="_blank">Let's build GPT: from scratch, in code [video] — Karpathy</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/rasbt/LLMs-from-scratch" target="_blank">LLMs from Scratch — volledig boek + code</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge any">Any</span> <a href="https://huggingface.co/learn/nlp-course/chapter1/1" target="_blank">Hugging Face NLP Course — Transformers begrijpen</a></li>
    </ul>
  </div>

  <!-- NEURAL NETWORK -->
  <div class="section" id="neural">
    <div class="section-header">
      <span class="section-tag">Fundamenten</span>
      <h2>Build your own Neuraal Netwerk</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://www.youtube.com/watch?v=aircAruvnKk" target="_blank">Neural Networks — 3Blue1Brown [video series]</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/karpathy/micrograd" target="_blank">micrograd: een tiny autograd engine in Python</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.deeplearning.ai/courses/deep-learning-specialization/" target="_blank">Deep Learning Specialization — Andrew Ng</a></li>
      <li><span class="lang-badge js">JavaScript</span> <a href="https://ml5js.org/" target="_blank">ml5.js: Neural Networks in de browser</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://cs231n.github.io/" target="_blank">CS231n: Convolutional Neural Networks — Stanford</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.tensorflow.org/tutorials" target="_blank">TensorFlow Tutorials — van beginners tot expert</a></li>
    </ul>
  </div>

  <!-- RECOMMENDER -->
  <div class="section" id="recommender">
    <div class="section-header">
      <span class="section-tag">Praktisch</span>
      <h2>Build your own Recommender Systeem</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://realpython.com/build-recommendation-engine-collaborative-filtering/" target="_blank">Recommendation Engine met Collaborative Filtering</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://towardsdatascience.com/how-to-build-a-movie-recommendation-system-67e321339109" target="_blank">Movie Recommender bouwen met Python</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://surprise.readthedocs.io/en/stable/getting_started.html" target="_blank">Surprise: Python library voor recommender systems</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.kaggle.com/learn/intro-to-machine-learning" target="_blank">Kaggle: Intro to Machine Learning — hands-on</a></li>
    </ul>
  </div>

  <!-- COMPUTER VISION -->
  <div class="section" id="vision">
    <div class="section-header">
      <span class="section-tag green">Computer Vision</span>
      <h2>Build your own Computer Vision Systeem</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://docs.ultralytics.com/" target="_blank">YOLOv8: Object Detection from Scratch met Ultralytics</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.youtube.com/watch?v=WQeoO7MI0Bs" target="_blank">Build a Face Recognition System with Python [video]</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://opencv.org/courses/" target="_blank">OpenCV: Computer Vision in Python — officiële cursus</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.tensorflow.org/tutorials/images/classification" target="_blank">Image Classification met TensorFlow & Keras</a></li>
      <li><span class="lang-badge js">JavaScript</span> <a href="https://teachablemachine.withgoogle.com/" target="_blank">Teachable Machine: Train een vision model in de browser</a> <span class="badge-new">New</span></li>
    </ul>
  </div>

  <!-- NLP -->
  <div class="section" id="nlp">
    <div class="section-header">
      <span class="section-tag">Taal & Tekst</span>
      <h2>Build your own NLP Pipeline</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://spacy.io/usage/spacy-101" target="_blank">spaCy 101: NLP pipeline bouwen met spaCy</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.nltk.org/book/" target="_blank">Natural Language Processing with Python — NLTK Book (gratis)</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://huggingface.co/docs/transformers/index" target="_blank">Transformers door Hugging Face — NLP met BERT & GPT</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://realpython.com/sentiment-analysis-python/" target="_blank">Sentiment Analysis in Python — stap voor stap</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://radimrehurek.com/gensim/auto_examples/index.html" target="_blank">Gensim: Topic Modeling en Word2Vec in Python</a></li>
    </ul>
  </div>

  <!-- AI AGENT -->
  <div class="section" id="agent">
    <div class="section-header">
      <span class="section-tag red">Trending</span>
      <h2>Build your own AI Agent</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://python.langchain.com/docs/modules/agents/" target="_blank">LangChain Agents: autonome AI-agents bouwen</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge py">Python</span> <a href="https://microsoft.github.io/autogen/" target="_blank">AutoGen: Multi-agent AI systemen van Microsoft</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://docs.crewai.com/" target="_blank">CrewAI: teams van AI-agents die samenwerken</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge ts">TypeScript</span> <a href="https://sdk.vercel.ai/docs/ai-sdk-core/agents" target="_blank">Vercel AI SDK — Agents met tools en geheugen</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.anthropic.com/claude" target="_blank">Claude API: Agents bouwen met tool use</a></li>
    </ul>
  </div>

  <!-- IMAGE GENERATION / DIFFUSION -->
  <div class="section" id="diffusion">
    <div class="section-header">
      <span class="section-tag red">Trending</span>
      <h2>Build your own Image Generation Model</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://huggingface.co/blog/annotated-diffusion" target="_blank">The Annotated Diffusion Model — Hugging Face blog</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.youtube.com/watch?v=a4Yfz2FxXiY" target="_blank">Stable Diffusion from Scratch in Python [video]</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/CompVis/stable-diffusion" target="_blank">Stable Diffusion — officiële repository + uitleg</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://keras.io/examples/generative/ddpm/" target="_blank">Denoising Diffusion met Keras — stap-voor-stap</a></li>
    </ul>
  </div>

  <!-- RAG -->
  <div class="section" id="rag">
    <div class="section-header">
      <span class="section-tag green">Praktisch</span>
      <h2>Build your own RAG & Vector Search</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://python.langchain.com/docs/use_cases/question_answering/" target="_blank">RAG systeem bouwen met LangChain + OpenAI</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge py">Python</span> <a href="https://docs.llamaindex.ai/en/stable/" target="_blank">LlamaIndex: Data framework voor LLM-applicaties</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.pinecone.io/learn/vector-database/" target="_blank">Vector Databases uitgelegd — Pinecone Learning Center</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.sbert.net/" target="_blank">Sentence Transformers: semantisch zoeken in Python</a></li>
      <li><span class="lang-badge ts">TypeScript</span> <a href="https://supabase.com/blog/openai-embeddings-postgres-vector" target="_blank">OpenAI Embeddings + Postgres als vector database</a></li>
    </ul>
  </div>

  <!-- VOICE AI -->
  <div class="section" id="voice">
    <div class="section-header">
      <span class="section-tag">Voice & Audio</span>
      <h2>Build your own Voice AI</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://github.com/openai/whisper" target="_blank">Whisper: Open-source spraakherkenning van OpenAI</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://speechbrain.github.io/" target="_blank">SpeechBrain: complete toolkit voor spraak-AI</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://coqui.ai/" target="_blank">Coqui TTS: tekst-naar-spraak model trainen</a></li>
      <li><span class="lang-badge js">JavaScript</span> <a href="https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API" target="_blank">Web Speech API: Voice AI direct in de browser</a></li>
    </ul>
  </div>

  <!-- SENTIMENT -->
  <div class="section" id="sentiment">
    <div class="section-header">
      <span class="section-tag">Analyse</span>
      <h2>Build your own Sentiment Analyse Tool</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://realpython.com/sentiment-analysis-python/" target="_blank">Sentiment Analysis in Python — complete guide</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://huggingface.co/blog/sentiment-analysis-python" target="_blank">Sentiment Analysis met Transformers — Hugging Face</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://www.kaggle.com/code/stoicstatic/twitter-sentiment-analysis-for-beginners" target="_blank">Twitter Sentiment Analysis voor beginners — Kaggle</a></li>
      <li><span class="lang-badge py">Python</span> <a href="https://textblob.readthedocs.io/en/dev/" target="_blank">TextBlob: eenvoudige NLP en sentimentanalyse in Python</a></li>
    </ul>
  </div>

  <!-- AI AUTOMATION -->
  <div class="section" id="automation">
    <div class="section-header">
      <span class="section-tag red">Trending</span>
      <h2>Build your own AI Automation</h2>
      <div class="section-line"></div>
    </div>
    <ul class="links-list">
      <li><span class="lang-badge py">Python</span> <a href="https://zapier.com/blog/ai-automation/" target="_blank">AI Automation uitgelegd + hoe je het zelf bouwt</a></li>
      <li><span class="lang-badge ts">TypeScript</span> <a href="https://n8n.io/" target="_blank">n8n: open-source AI workflow automation</a> <span class="badge-new">New</span></li>
      <li><span class="lang-badge py">Python</span> <a href="https://docs.prefect.io/" target="_blank">Prefect: AI-workflows orkestreren in Python</a></li>
      <li><span class="lang-badge any">Any</span> <a href="https://make.com" target="_blank">Make (Integromat): no-code AI automation bouwen</a></li>
      <li><span class="lang-badge ts">TypeScript</span> <a href="https://www.inngest.com/" target="_blank">Inngest: Event-driven AI workflows voor developers</a></li>
    </ul>
  </div>

  <!-- CONTRIBUTE -->
  <div class="contribute">
    <h3>Bijdragen</h3>
    <p style="color: var(--muted); font-size: 13px;">Ken jij een goede tutorial die hier nog niet bij staat? Stuur een suggestie via <a href="mailto:hello@taskly.app">hello@taskly.app</a>. Taskly is een open kennisplatform — gebouwd voor iedereen die AI wil begrijpen door het zelf te maken.</p>
  </div>

  <footer>
    <p>© 2026 <strong style="color: var(--text);">Taskly</strong> — Het AI kennisplatform. Geïnspireerd door <a href="https://github.com/codecrafters-io/build-your-own-x" target="_blank">build-your-own-x</a>.</p>
    <p style="margin-top: 6px;">Alle links leiden naar externe, publiek beschikbare tutorials. Taskly claimt geen eigenaarschap over gelinkte content.</p>
  </footer>

</div>
</body>
</html>

