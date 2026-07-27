# Human-Agent Workforce OS

Human-Agent Workforce OS ("Orchestra") is a workspace for routing and governing work across people, AI agents, and hybrid teams. It evaluates each task, recommends an execution route, applies organizational policies, and records approvals, outcomes, and audit activity.

![Orchestra dashboard](examples/d1/dashboard.png)

[Watch the demo video](examples/d1/demo.mp4)

## Features

- Route work to a human, an AI agent, or a hybrid team.
- Evaluate tasks by risk, sensitivity, judgment, urgency, and verifiability.
- Require accountable human approval for governed work.
- Manage workforce members, permissions, and routing policies.
- Track task progress, agent runs, tool calls, evaluations, and audit events.
- Run governed task agents through Amazon Bedrock.
- Store workspace data in a local D1-compatible SQLite database during development.

## Technology

- React 19 and Next.js 16
- Vinext and Vite
- TypeScript
- Cloudflare's local Worker and D1-compatible development runtime
- Drizzle ORM
- Amazon Bedrock for optional AI-agent execution

The local setup does not require OpenAI Sites or OpenAI hosting.

## Requirements

- Node.js 22.13 or newer
- npm 11 or newer
- Amazon Bedrock credentials only if you want to execute AI agents

Check your installed versions:

```powershell
node --version
npm --version
```

## Local setup

1. Clone your repository and enter the project directory:

   ```powershell
   git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
   cd YOUR-REPOSITORY
   ```

2. Install dependencies:

   ```powershell
   npm ci
   ```

3. If you want Amazon Bedrock integration, create a `.dev.vars` file in the project root:

   ```dotenv
   AWS_ACCESS_KEY_ID=your_access_key
   AWS_SECRET_ACCESS_KEY=your_secret_key
   AWS_REGION=us-east-1
   BEDROCK_MODEL_ID=amazon.nova-pro-v1:0
   ```

   Optional temporary credentials and guardrail settings are also supported:

   ```dotenv
   AWS_SESSION_TOKEN=your_session_token
   BEDROCK_GUARDRAIL_ID=your_guardrail_id
   BEDROCK_GUARDRAIL_VERSION=your_guardrail_version
   ```

4. Start the development server:

   ```powershell
   npm run dev
   ```

5. Open the local address printed in the terminal, normally [http://localhost:5173](http://localhost:5173).

The dashboard and seeded demonstration data work without AWS credentials. Bedrock-backed agent execution remains unavailable until valid credentials are configured.

## Available commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the local development server |
| `npm run build` | Create a production build |
| `npm run start` | Start the built application |
| `npm run lint` | Check the code with ESLint |
| `npm test` | Build the app and run the rendered HTML test |
| `npm run db:generate` | Generate Drizzle database migrations |

## Local data

Development data is stored beneath `.wrangler/` and is excluded from Git. The application creates its tables and seed data automatically when the workspace is first loaded.

To reset only the local development database, stop the server and remove the `.wrangler` directory. The next start creates a fresh database.

## Project structure

```text
app/        User interface and API routes
db/         Database schema, initialization, and seed data
drizzle/    Generated database migrations
lib/        Routing, governance, access, and Bedrock logic
public/     Static assets
tests/      Application tests
worker/     Local Worker entry point and runtime bindings
```

## Security

- Never commit `.dev.vars` or any other credential file.
- Use short-lived AWS credentials where possible.
- Rotate credentials immediately if they appear in Git history or terminal output.
- Review the active governance policies before allowing agents to operate on sensitive work.
