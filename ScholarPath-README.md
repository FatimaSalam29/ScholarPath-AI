# ScholarPath AI

ScholarPath AI is a web application for evaluating scholarships, fellowships, and other educational opportunities against a student's academic profile. It organizes opportunity requirements, checks eligibility, identifies profile matches and gaps, and produces an application preparation plan.

> **Important:** ScholarPath AI is an advisory tool, not an official eligibility decision or a guarantee of admission, funding, or selection. Always confirm requirements with the opportunity provider.

## Problem Statement

Information about scholarships and academic opportunities can be spread across lengthy documents and websites. Students may find it difficult to identify hard eligibility requirements, compare those requirements with their own background, and distinguish eligibility conditions from documents they still need to prepare.

ScholarPath AI aims to make this review more structured and transparent by separating opportunity analysis, eligibility checks, profile matching, application planning, and a final verification pass.

## Features

- **Opportunity analysis:** Extracts structured information such as organization, opportunity type, deadlines, funding, location, eligibility criteria, and required documents from supplied opportunity text.
- **Eligibility assessment:** Evaluates hard requirements and classifies criteria using `MET`, `NOT_MET`, and `NEEDS_VERIFICATION` statuses.
- **Student profile matching:** Compares opportunity criteria with the education, skills, experience, projects, and other evidence provided in a student profile.
- **Application planning:** Organizes required application materials and preparation actions.
- **Verification stage:** Reviews the analysis for unsupported claims, missing information, and inconsistencies.
- **Document text extraction:** Accepts text/Markdown input and supports PDF text extraction through the backend.
- **Sample data and demo results:** Includes sample student profiles, opportunities, and precomputed analysis data.
- **Deterministic eligibility logic:** Uses code-based checks for eligibility criteria rather than relying entirely on generated text.
- **Optional AI analysis:** Uses OpenRouter when an API key is configured; the application also contains a deterministic/sample evaluation path.

## Analysis Pipeline

```text
Opportunity text + Student profile
              |
              v
      1. Opportunity Analyzer
              |
              v
      2. Eligibility Analyzer
              |
              v
       3. Profile Match Agent
              |
              v
       4. Application Planner
              |
              v
       5. Verification Agent
              |
              v
        Structured result
```

The eligibility analyzer performs deterministic checks. The other analysis stages can use the configured AI client and their implemented fallback behavior.

## Technology Stack

### Frontend
- React 19
- TypeScript
- Vite
- Tailwind CSS 4
- Lucide React
- Motion

### Backend
- Node.js
- Express
- TypeScript
- tsx

### AI and document processing
- OpenAI SDK configured to communicate with OpenRouter
- Google GenAI SDK is included in the dependencies and a Gemini client utility exists in `server/lib/gemini.ts`; the current pipeline orchestrator uses the OpenRouter client.
- `pdf-parse` for PDF text extraction
- dotenv for environment configuration

## Project Structure

```text
scholarpath-ai/
├── docs/
│   └── PRD.md
├── server/
│   ├── agents/
│   │   ├── applicationPlanner.ts
│   │   ├── eligibilityAnalyzer.ts
│   │   ├── opportunityAnalyzer.ts
│   │   ├── orchestrator.ts
│   │   ├── profileMatchAgent.ts
│   │   ├── types.ts
│   │   └── verificationAgent.ts
│   ├── lib/
│   │   ├── gemini.ts
│   │   └── openrouter.ts
│   └── sampleData.ts
├── src/
│   ├── components/
│   ├── context/
│   ├── services/
│   ├── App.tsx
│   ├── index.css
│   ├── main.tsx
│   └── types.ts
├── tmp/
│   └── test_fellowship.json
├── .env.example
├── .gitignore
├── index.html
├── package.json
├── package-lock.json
├── server.ts
├── tsconfig.json
└── vite.config.ts
```

## Requirements

- Node.js (a current LTS release is recommended)
- npm
- An OpenRouter API key for AI-powered analysis. The deterministic/sample evaluation path can be used without configuring a key, subject to the application's implemented fallback behavior.

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR-USERNAME/scholarpath-ai.git
   cd scholarpath-ai
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a local environment file:

   ```bash
   cp .env.example .env
   ```

4. Open `.env` and add your own OpenRouter API key:

   ```env
   OPENROUTER_API_KEY=your_openrouter_api_key
   OPENROUTER_MODEL=openai/gpt-oss-20b:free
   PORT=3000
   ```

   Keep `.env` private. Do not commit or publish it. The repository's `.env.example` should contain placeholders only.

## Run Locally

Start the application in development mode:

```bash
npm run dev
```

The Express server starts on port `3000` by default and attaches Vite middleware for the frontend. Open `http://localhost:3000`.

## Available Scripts

| Command | Purpose |
|---|---|
| `npm run dev` | Starts Express with Vite development middleware |
| `npm run build` | Builds the frontend into `dist/` |
| `npm start` | Starts the server; set `NODE_ENV=production` to serve the built frontend |
| `npm run lint` | Runs the TypeScript compiler in no-emit mode |

For a production-style local run:

```bash
npm run build
NODE_ENV=production npm start
```

## API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/health` | Returns server status and configured analysis mode |
| `GET` | `/api/samples` | Returns sample profiles, opportunities, and precomputed demo analysis |
| `POST` | `/api/extract-document` | Extracts text from supported document uploads, including PDFs |
| `POST` | `/api/analyze` | Runs the five-stage opportunity analysis pipeline |

## Usage

1. Start the application.
2. Review or enter a student profile.
3. Provide the text of a scholarship, fellowship, or other educational opportunity.
4. Run the analysis.
5. Review eligibility results, profile matches, gaps, application preparation actions, and verification notes.
6. Confirm important requirements with the official opportunity provider before acting on the result.

## Documentation

- [`docs/PRD.md`](docs/PRD.md) contains the product requirements document.
- `server/agents/` contains the analysis stages and shared types.
- `server/sampleData.ts` contains sample data used by the application.

## Configuration and Security

- Store API credentials only in a local `.env` file or a secure deployment environment.
- Never commit `.env`, access tokens, passwords, or private credentials.
- Keep `.env.example` limited to variable names and safe placeholder values.
- Review sample and test data before publishing if it contains personal or confidential information.
- Do not treat generated analysis as authoritative admissions or funding advice.

## Current Limitations

- Results depend on the accuracy and completeness of the opportunity text and student profile supplied.
- Some eligibility conditions may require manual interpretation or confirmation with the provider.
- AI-backed stages depend on external provider availability and configuration.
- Deterministic/sample evaluation is a fallback and is not equivalent to a full AI-backed analysis.
- The application does not submit applications on a student's behalf.

## Future Improvements

- Add source URLs and evidence citations for extracted requirements.
- Add automated tests for eligibility edge cases and pipeline outputs.
- Improve validation and user feedback for uploaded documents and profile data.
- Add additional configurable AI providers to the active pipeline.
- Improve opportunity comparison, filtering, and ranking.
- Add deployment and monitoring documentation.

## License

No license file was included in the supplied project archive. Add a `LICENSE` file before presenting the project as open source, and choose a license that matches the project's intended reuse terms.
