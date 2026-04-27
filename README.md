# A14_Project_Thesis
**CodeAssista: AI-Powered Code Generation, Debugging, and Translation Platform**

**Authors**

R. Tarun (Y22ACS547)

M. Ranjith Kumar (Y22ACS544)

P. Gowthami (Y22ACS530)

Sk. Khaja Khayuam (Y22ACS557)


**Overview**

CodeAssista is a production-ready, full-stack web application that delivers AI-powered programming assistance to software developers and computer science students. Built on the MERN stack — MongoDB, Express.js, React.js, and Node.js — the platform integrates Google's Gemini 2.5 Flash large language model to provide intelligent, context-aware responses across three structured operational modes: Code Generation, Code Debugging, and Code Translation.
Key innovation: Structured prompt engineering system with mode-specific templates that consistently extract high-quality, focused AI responses — eliminating the need for users to craft their own prompts.

**Project Components**

**1.Code Generation Module**

Accepts natural language descriptions of programming tasks
Produces clean, well-structured, executable code
Supports any programming language based on user description

**2.Code Debugging Module**

Analyzes user-supplied code using a four-step sequential prompt
Identifies syntactic and logical errors
Explains each issue in plain language
Returns a fully corrected version of the code

**3.Code Translation Module**

Translates programs between five popular programming languages: Python, Java, JavaScript, C, and C++
Maintains the same logic in the translated output
Dedicated language selector dropdown in the UI

**4.Authentication System**

JWT-based stateless authentication
User registration with full name, username, email, and password
Passwords cryptographically hashed using bcryptjs (10 salt rounds)
All AI routes protected by JWT middleware


**System Architecture**

CodeAssista follows a two-tier client-server architecture with a third-party AI service integration:
┌─────────────────────────────────────────────────────────┐
|                                        
│           PRESENTATION TIER (Frontend)                  |  
│
│  React.js v19 + React Router DOM v7 + Axios       
│
│  Home | Login | Signup | CodeAssistant Pages            │
└────────────────────┬────────────────────────────────────┘
                     │ HTTP/REST (JSON)
┌────────────────────▼────────────────────────────────────┐

│          APPLICATION TIER (Backend)                     │

│  Node.js v20 + Express.js v5                           │

│  authRoutes | aiRoutes | authMiddleware                │

│  JWT Verification + Prompt Engineering                 │
└──────────┬─────────────────────────────┬───────────────┘
           │ Mongoose ODM                │ SDK Call
┌──────────▼──────────┐    ┌────────────▼────────────────┐
│   DATA TIER          │    │   AI TIER                   │
│   MongoDB Atlas      │    │   Google Gemini 2.5 Flash   │
│   User Collection    │    │   Prompt → Response         │
└──────────────────────┘    └─────────────────────────────┘

**Key Architecture Notes:**

Stateless JWT — no server-side sessions

Gemini API key never exposed to the browser

CORS enabled for frontend origin

Browser communicates only with Express.js server

**Technologies Used**

**Frontend**

React.js 19.x — Component-based UI library

React Router DOM 7.x — Client-side routing with auth guard

Axios 1.x — HTTP client with centralized base URL config

react-markdown 10.x — Renders AI markdown responses

Custom CSS — Glassmorphism design, gradient backgrounds

**Backend**

Node.js 20.x LTS — JavaScript runtime

Express.js 5.x — Web application framework

MongoDB Atlas — NoSQL cloud database

Mongoose 9.x — MongoDB ODM with schema validation

jsonwebtoken 9.x — JWT signing and verification (HS256)

bcryptjs 3.x — Password hashing

@google/generative-ai 0.24.x — Gemini SDK

dotenv 17.x — Environment variable management

cors 2.x — Cross-origin request support


**Key Features**

**AI-Powered Features**

Code Generation — Natural language to clean, executable code

Code Debugging — Four-step structured analysis: identify, explain, correct

Code Translation — Converts code between Python, Java, JavaScript, C, and C++

Structured Prompt Engineering — Mode-specific templates for consistent output quality

**Security Features**

JWT Authentication — Secure stateless sessions (1-hour expiry, HS256)

bcrypt Password Hashing — Salt rounds ≥ 10 per OWASP guidelines

Protected Routes — Client-side auth guard + server-side JWT middleware

API Key Security — Gemini API key stored server-side only, never sent to browser

Input Validation — HTTP 400 for missing or invalid fields

**Performance Benchmarks**

**Metric**                            **Target**  **Observed**

Login API response                    < 300 ms    ~120 ms

Signup API response                   < 500 ms    ~180 ms

JWT middleware overhead               < 10 ms     ~2 ms

AI generation (simple)                < 10 s      ~3–5 s

AI debugging (medium)                 < 20 s      ~8–12 s

React First Contentful Paint          < 2 s       ~0.8 s

**API Endpoints**

**Method**    **Endpoint**        **Auth**      **Description**

POST          /api/auth/signup      None          Register new user

POST          /api/auth/login       None          Authenticate, returns JWT

POST          /api/ai/generate      BearerJWT      Code generation / debugging / translation

GET           /api/protected        Bearer JWT      JWT middleware test route

GET           /                     None            Health check


The /api/ai/generate endpoint accepts { prompt, mode, targetLanguage } where mode is generate, debug, or convert.



**Installation & Setup**

**Prerequisites**

Node.js 20.x or higher

MongoDB 6.0 or higher (or MongoDB Atlas account)

npm package manager

Google Gemini API key (from Google AI Studio)

**Backend Setup**

bash

# Clone the repository
git clone https://github.com/yourusername/codeassista-backend.git
cd codeassista-backend

# Install dependencies
npm install

# Create .env file
cat > .env << EOF
PORT=5000
MONGO_URI=mongodb://localhost:27017/codeassista
JWT_SECRET=your_super_secret_jwt_key_minimum_32_characters
GEMINI_API_KEY=your_google_gemini_api_key_here
EOF

# Start development server
npm run dev

# Start production server
npm start
Frontend Setup
bash# Clone the repository
git clone https://github.com/yourusername/codeassista-frontend.git
cd codeassista-frontend

# Install dependencies
npm install

# Start development server (runs on port 3000)
npm start

# Build for production
npm run build
Database Setup
bash# Start MongoDB locally
mongod

# Or use MongoDB Atlas (cloud) — replace MONGO_URI in .env with Atlas connection string

Environment Variables
Backend (.env)
env# Server
PORT=5000
NODE_ENV=development

# Database
MONGO_URI=mongodb://localhost:27017/codeassista

# Authentication
JWT_SECRET=your_super_secret_jwt_key_minimum_32_characters

# Google Gemini AI
GEMINI_API_KEY=your_google_gemini_api_key_here

⚠️ Never commit your .env file. It is excluded from version control via .gitignore.


Project Structure
backend/
├── middleware/
│   └── authMiddleware.js     ← JWT verification, protects /api/ai/* routes
├── models/
│   └── User.js               ← Mongoose schema: fullName, username, email, password
├── routes/
│   ├── authRoutes.js         ← POST /signup, POST /login
│   └── aiRoutes.js           ← POST /generate (mode: generate|debug|convert)
├── .env                      ← MONGO_URI | JWT_SECRET | GEMINI_API_KEY | PORT
├── package.json
└── server.js                 ← App init, middleware, routes, DB connect, listen

frontend/src/
├── api/
│   └── axios.js              ← Centralized Axios instance (baseURL: localhost:5000)
├── pages/
│   ├── Home.js / Home.css    ← Landing page with hero, features, tech stack
│   ├── Login.js / Login.css  ← Split-screen login form with JWT storage
│   ├── Signup.js / Signup.css← Registration card with hover animation
│   └── CodeAssistant.js/.css ← 3-mode AI interface: generate | debug | convert
└── App.js                    ← Routes + auth guard (/app protected route)

Prompt Engineering System
CodeAssista uses three independently optimized prompt templates:
Code Generation — Quality-focused prefix primes the model for clean output:
Generate clean, correct, and well-structured code for the following request:
{user_prompt}
Code Debugging — Four-step chain-of-thought sequence for thorough analysis:
You are an expert software developer.
TASK:
1. Analyze the following code.
2. Identify all errors and issues.
3. Explain each error clearly.
4. Provide a corrected version of the code.
CODE: {user_code}
Code Translation — Explicit constraints ensure clean, logic-preserving output:
You are a professional software engineer.
Convert the following code into {targetLanguage}.
Rules: 1. Maintain the same logic. 2. Write clean and correct code. 3. Only return the converted code.
CODE: {user_code}

Testing
All 20 test cases passed during validation:
bash# Run frontend tests
npm run test

# Test backend endpoints via Postman
# Import the collection or use the API endpoints listed above
Test coverage includes: User registration/login, JWT middleware (valid/invalid/missing tokens), all three AI modes, input validation, frontend routing guards, mode state clearing, and password hash storage verification.

Research Contribution
This project demonstrates how structured prompt engineering and stateless JWT authentication can be combined with a modern MERN stack to deliver a focused, production-quality AI developer tool. Key contributions:

Mode-specific prompt templates based on LLM prompting research (Chen et al., Wei et al.)
JWT implementation aligned with OWASP security guidelines (bcrypt, HS256)
Two-tier stateless architecture enabling horizontal scalability

Department of Computer Science and Engineering, Bapatla Engineering College

Future Enhancements

Conversation History — Multi-turn sessions using Gemini's startChat() API with MongoDB persistence
Syntax Highlighting — Prism.js/highlight.js integration with copy-to-clipboard button
File Upload for Debugging — Multer-based file ingestion for multi-file codebases
Expanded Language Support — Rust, Go, TypeScript, Kotlin, Swift, Ruby
User Dashboard — Personal interaction history with bookmarking and re-run capability
Admin Analytics — Usage metrics dashboard with Chart.js visualization
Production Hardening — Rate limiting, CORS origin restriction, refresh token rotation, HTTPS


Acknowledgements
We would like to thank:

Mr. K. Arun Babu — Project Guide and Mentor, Assistant Professor, Dept. of CSE
Dr. M. Rajesh Babu — Head of Department, CSE
Dr. C. Prakasa Rao & Dr. R. Venkata Lakshmi — Project Coordinators
Dr. B. Srinivasarao — Principal, Bapatla Engineering College
Google Cloud — For Gemini AI API access
MongoDB Atlas — For database hosting


Built with ❤️ by CodeAssista Team | Bapatla Engineering College | 2025-2026
