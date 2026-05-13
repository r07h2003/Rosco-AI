Rosco AI — iMessage Chief of Staff
Rosco is a personal AI chief of staff that lives in iMessage. It manages my emails, tracks job opportunities, monitors my calendar, and sends daily briefings — all through a simple text conversation. Built on Claude with persistent memory that learns my preferences over time.

Why I Built It
I was constantly switching between Gmail, my calendar, and job boards — and still missing things that mattered. Important emails were slipping through. Application deadlines were passing before I even saw the listing. And every AI tool I tried reset the moment a conversation ended, so I was re-explaining myself every single time.
I wanted something that knew me, remembered everything, and felt natural to use. Nothing is more natural than iMessage. So I built Rosco there.

What It Does

Summarizes my inbox daily and flags anything I may have missed
Sends a list of internship and job postings from the last 24 hours every morning
Answers questions about my calendar without me opening Google Calendar
Remembers context across conversations — new chat, same Rosco


Architecture
You (iMessage)
     │
     ▼
SendBlue ──────────────────────► Convex (Backend + DB)
                                        │
                                        ▼
                               Composio Integration Layer
                               Gmail · Google Calendar · GitHub
                                        │
                                        ▼
                               Claude + Boop Agent SDK
                               (Memory · Behavior · Response)
                                        │
                                        ▼
                               Response via SendBlue ──► You (iMessage)

Tech Stack
ToolRoleWhy I chose itSendBlueiMessage API layerOnly reliable way to send/receive iMessages programmaticallyConvexBackend + databaseServerless, real-time, handles webhook routing and memory storageComposioIntegration layerConnected Gmail, Calendar, and GitHub without building direct OAuth flows for eachClaude (Anthropic)AI modelBest-in-class instruction following and context handlingBoop Agent SDKMemory + behavior layerOpen-source Claude wrapper with persistent memory — saved building this from scratch

Key Decisions
Composio over direct API integrations
Building OAuth flows for Gmail, Calendar, and GitHub separately would have taken weeks. Composio abstracted all of that into one integration layer, letting me focus on the product behavior instead of plumbing.
Boop Agent SDK over custom memory
I evaluated the SDK's behavioral parameters and response consistency and decided adopting it was smarter than engineering my own memory layer. The output quality matched what I needed out of the box.
iMessage over a custom app
The whole point was friction removal. Building a standalone app would have added the exact friction I was trying to eliminate. iMessage is already open on my phone.

What Broke
Webhook pipeline failure
Initially Rosco wasn't returning any responses at all after setup. The issue was in the webhook connection between SendBlue and Convex — the pipeline wasn't firing correctly. Tracing the request flow and fixing the webhook configuration resolved it.
Memory layer corruption
The memory layer built on Convex was getting corrupted, which caused Rosco to break mid-conversation and lose context entirely. The issue was in how memory was being written and read from the Convex database — fixing the schema and storage logic resolved it.

What's Next

Custom behavioral layer — replacing the Boop Agent SDK over time with a personally tuned memory and response system that learns my patterns, communication style, and priorities at a deeper level than a general-purpose SDK allows
Proactive nudges — Rosco flags deadlines and important emails before I ask, rather than waiting for me to prompt it
Application pipeline tracker — integrations with Lever, Greenhouse, and Workday so Rosco can surface application statuses across platforms in one daily briefing, no manual checking
LinkedIn integration — pull relevant job postings, unread messages, and connection activity directly into Rosco's daily digest
Canvas integration — assignment deadlines, grade updates, and course announcements surfaced automatically so nothing slips through
Unified deadline tracker — one layer that aggregates deadlines across job applications, Canvas assignments, and calendar events and flags anything within 48 hours


Credits
Built with Boop Agent SDK by raroque — an open-source Claude agent framework with persistent memory.

Built by Rohan Harishchandrakar · Portfolio
