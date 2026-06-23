# tiktok-swarm

tiktok-swarm — Autonomous Video Pipeline
A 6-agent TypeScript system that takes a topic and produces a fully rendered, ready-to-post TikTok video — end to end, no human in the loop.

Pipeline
Topic Input
    │
    ▼
[Researcher]     → pulls trending context, hooks, and facts
    │
    ▼
[Scriptwriter]   → writes timed script with hook, body, CTA
    │
    ▼
[Voice Producer] → generates voiceover via ElevenLabs
    │
    ▼
[Visual Director]→ selects and sequences stock footage via Pexels API
    │
    ▼
[Composer]       → assembles video + audio via Remotion
    │
    ▼
[Reviewer]       → scores output; loops back if quality threshold not met
    │
    ▼
 Final .mp4 → posted or queued for upload

Agents communicate through a typed PipelineState object. The Reviewer agent can trigger revision loops up to a configurable max before accepting output.

Tech Stack
|Layer    |Technology      |
|---------|----------------|
|Language |TypeScript      |
|Voice    |ElevenLabs API  |
|Footage  |Pexels API      |
|Rendering|Remotion        |
|Scheduler|cron (Linux VPS)|
|Runtime  |Node.js         |

Getting Started
git clone https://github.com/Vraj4592/tiktok-swarm
cd tiktok-swarm
npm install

Create a .env file:
ELEVENLABS_API_KEY=your_key
PEXELS_API_KEY=your_key
ANTHROPIC_API_KEY=your_key

Run manually:
bash
npm run swarm:run -- --topic "your topic here"

Or schedule via cron:
bash
# runs daily at 9am
0 9 * * * cd /home/user/tiktok-swarm && npm run swarm:run

Project Structure
src/
├── agents/
│   ├── researcher.ts
│   ├── scriptwriter.ts
│   ├── voiceProducer.ts
│   ├── visualDirector.ts
│   ├── composer.ts
│   └── reviewer.ts
├── state.ts                  # PipelineState type + shared contract
├── orchestrator.ts           # Agent loop + revision logic
└── index.ts                  # Entry point

Why I built this
Content creation at scale is a systems problem. This pipeline treats each stage of video production as an isolated, testable agent with clear inputs and outputs — the same way you’d architect any distributed system.