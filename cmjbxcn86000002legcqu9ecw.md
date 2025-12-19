---
title: "Elimu.ai - A Production-Ready Agentic AI SaaS"
seoTitle: "Build AI SaaS with Next.js, Supabase & Voice AI"
seoDescription: "Learn how to build a production-ready AI SaaS with Next.js, Supabase, Clerk, and voice AI agents  step-by-step code examples included."
datePublished: Thu Dec 18 2025 21:00:19 GMT+0000 (Coordinated Universal Time)
cuid: cmjbxcn86000002legcqu9ecw
slug: elimuai-a-production-ready-agentic-ai-saas
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766043262961/b6f524b0-7769-4da8-b8ec-f34319f861e3.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1766052861026/3815198d-9d5e-40d8-8578-a88680e27444.jpeg
tags: ai, artificial-intelligence, javascript, machine-learning, full-stack, serverless, nextjs, full-stack-development, tailwind-css, supabase, clerkdev, shadcn-ui, rag, voiceai, vapi-voicebots

---

Agentic AI systems where LLM-powered agents autonomously pursue user goals by planning and invoking tools is transforming software design. Today’s startups are rushing to embed AI agents in their products, but many early demos fail to survive real-world .Acheiving durability at scale requires engineering rigor: **stateful orchestration, fault tolerance, multi-tenant isolation, and robust monitoring**. In production, agentic systems must handle unreliable tools and humans, non-deterministic LLM outputs, and evolving data across customers. By following well-tested patterns, AI/ML engineers and senior developers can turn prototypes into resilient, scalable SaaS .This guide unpacks key concepts, architecture principles, and operational practices for building advanced agentic AI services (with paths for intermediate devs to learn along the way).

### [Elimu.ai](https://github.com/josaphat12-tech/Elimu.ai/) Agentic AI Concepts & Flow

[**Elimu.ai**](https://github.com/josaphat12-tech/Elimu.ai/) is a full-stack LMS SaaS with AI-powered voice tutors, authentication, and subscription billing. It uses **Next.js** for the app framework, **Clerk** for auth and billing, **Supabase** for the backend , **Vapi.ai** for voice AI agents, **shadcn/ui + Tailwind CSS** for UI components, and **Sentry** for error monitoring. Together these tools let us rapidly build a scalable, interactive learning platform. This system revolves around autonomous agents that use LLMs and external tools to accomplish goals. In such a system, a Goal is a user’s high-level request; the Agent (LLM) breaks it into steps, and Tools are services or APIs that execute actions on .For example, a <mark>bug triaging</mark> agent might use tools to query a bug tracker database or send notifications. Advanced architectures may use **multi-agent** setups (agents invoking other sub-agents or specialized modules), but at the core, each agent follows a control loop of plan–act–observe.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766045465747/13f040a4-f462-47d3-b8e9-fdd96b4fdb9c.jpeg align="center")

### **Tech Stack Highlights**

* **Clerk** – Unified auth and user management (sign-up, login, social Oauth, subscriptions)
    
* **Supabase** – PostgreSQL backend with instant APIs and real-time updates
    
* **Vapi.ai** – Voice AI platform to create conversational agents (speech-to-text & TTS)
    
* **Shadcn/ui + Tailwind CSS** – Pre-built React components and utility-first CSS for fast UI development
    
* **Sentry** – Real-time error tracking to monitor crashes and performance
    

### Authentication with Clerk

First,here is a basic Clerk setup schema in your Next.js app. In `app/layout.tsx` (or `_app.tsx`), wrap your app with the `<ClerkProvider>` (with your publishable key). Then create protected pages or components using Clerk’s hooks. For example, a simple **<mark>SignIn</mark>** page could be:

```typescript
'use client';
import { SignIn, useUser } from '@clerk/nextjs';

export default function SignInPage() {
  const { isSignedIn } = useUser();
  // If not signed in, show Clerk's sign-in form
  if (!isSignedIn) {
    return <SignIn />;
  }
  // If signed in, redirect or show a message
  return <div className="p-4">Welcome back, user!</div>;
}
```

You can also use **Clerk’s components** for sign-out or conditional rendering. For example:

```typescript
import { SignedIn, SignedOut, SignOutButton, SignInButton } from '@clerk/nextjs';

export function Navbar() {
  return (
    <nav className="p-4 flex justify-end space-x-4">
      <SignedIn>
        <SignOutButton>Sign Out</SignOutButton>
      </SignedIn>
      <SignedOut>
        <SignInButton>Sign In</SignInButton>
      </SignedOut>
    </nav>
  );
}
```

**Clerk** makes it easy to guard routes. In the Next.js App Router, you can use `await currentUser()` in a server component or middleware to require authentication. **Clerk** also handles billing and subscription plans out-of-the-box (you can define plans in Clerk’s dashboard and restrict access by subscription tier). Clerk is “a unified platform for authentication, user management, and billing”, so adding <mark>signup/login</mark> flows only takes a few lines of code.

### Database & Session History with Supabase

Use **Supabase** to store users’ learning **sessions** and **bookmarks**. First, initialize the client (e.g. in `lib/supabase.ts`):

```typescript
import { createClient } from '@supabase/supabase-js';

export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
);
```

With that in place, you can insert or query the `sessions` and `bookmarks` tables. For example, to save a completed tutoring session:

```typescript
// Example: save a session record for the current user
async function saveSession(userId: string, transcript: string) {
  const { data, error } = await supabase
    .from('sessions')
    .insert([{ user_id: userId, transcript, created_at: new Date() }]);
  if (error) throw error;
  return data;
}
```

To fetch a user’s past sessions or bookmarked tutors:

```typescript
// Fetch all sessions for a user
const { data: sessions } = await supabase
  .from('sessions')
  .select('*')
  .eq('user_id', userId);

// Add or remove a bookmark
await supabase
  .from('bookmarks')
  .upsert({ user_id: userId, tutor_id: selectedTutorId });
```

With **Supabase**’s real-time capabilities and simple Postgres interface, you can quickly build features like session history lists and the ability to bookmark favorite tutors. **Supabase** is ***“an open-source backend-as-a-service platform that provides instant APIs, real-time subscriptions, authentication, storage, and a PostgreSQL database”*** so you get a full backend with minimal setup.

### Voice AI with **Vapi.ai**

At the heart of Elimu.ai are AI voice tutors powered by **Vapi**. **Vapi** lets us program **voice agents** that carry on natural conversations (**using your choice of LLM like GPT-4**) and speak in a selected voice. For example, to start a tutoring call using the Web SDK:

```typescript
import Vapi from '@vapi-ai/web';

const vapi = new Vapi(process.env.NEXT_PUBLIC_VAPI_WEB_TOKEN!);

async function askTutor(question: string) {
  const { data } = await vapi.start({
    model: { provider: 'openai', model: 'gpt-4' },
    messages: [
      { role: 'system', content: 'You are a friendly AI math tutor.' },
      { role: 'user', content: question },
    ],
    voice: 'en-US-Wavenet-D', // choose a voice
  });
  console.log('Tutor says:', data.answer);
}
```

This code sends the user’s question to **Vapi’s API**, which uses **GPT-4** to generate a response and returns it (*with text-to-speech data if you embed audio*). According to Vapi’s docs, this lets you “*build voice AI agents*” that can make and receive calls. In fact, **Elimu.ai** follows the approach of building a “voice-first AI tutor” using **Vapi** and **Next.js**

You can then render the audio or stream it back to the user in the UI (for example, by using the Web Audio API or a simple `<audio>` element). Optionally, you can listen for tool-calls or events from **Vapi** to handle client-side actions as shown in their examples

### UI with Shadcn/UI and Tailwind CSS

For a polished frontend, we use **shadcn/ui** (Radix + Tailwind) components along with Tailwind classes. For example, a tutor card component might look like:

```typescript
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';

export function TutorCard({ tutor }: { tutor: { name: string; desc: string } }) {
  return (
    <div className="p-4 border rounded-lg shadow">
      <h3 className="text-xl font-semibold">{tutor.name}</h3>
      <p className="mt-2 text-gray-600">{tutor.desc}</p>
      <Button className="mt-4">Start Session</Button>
    </div>
  );
}

// Example: a search bar using shadcn input & button
export function TutorSearch() {
  return (
    <div className="flex space-x-2">
      <Input placeholder="Search tutors..." className="flex-grow" />
      <Button>Go</Button>
    </div>
  );
}
```

These components use simple **JSX** and Tailwind **CSS** classes for layout and styling. **Shadcn/ui** provides ready-made components (like `Button`, `Input`, etc.), making the code concise. T**ailwind’**s utility classes (e.g. `p-4`, `bg-gray-100`) handle the look-and-feel. This setup ensures a responsive, modern **UI** without writing much custom **CSS**. You could plug in more components (`dialogs`, `cards`, `menus`, etc.) from `shadcn/`ui as needed to build dashboards and modals.

### Error Monitoring with Sentry

To catch and monitor any runtime errors or performance issues, integrate **Sentry**. In your **Next.js** project, install `@sentry/nextjs` and add an initializer (for example, in `sentry.client.config.js` or at the top of your code). For instance:

```typescript
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN!,
  tracesSampleRate: 1.0, // adjust sampling as needed
});
```

Now **Sentry** will automatically capture unhandled errors and performance traces. You can also manually report errors:

```typescript
try {
  // ...some code that might fail...
} catch (err) {
  Sentry.captureException(err);
}
```

This ensures you have visibility into issues in production. As the project README.md notes, **Sentry** is “an error tracking and performance monitoring tool that helps developers fix bugs faster”. With **Sentry** set up, any crashes in your **Next.js** app (client or server) will appear in your **Sentry** dashboard, complete with stack traces.

### Multi-Tenant SaaS Challenges

Building an AI agent for a **SaaS platform** adds layers of complexity. In a multi-tenant environment (hundreds or thousands of customers), the system must handle variability and isolation that single-tenant demos never face. Key challenges include:

* **Data & Context Isolation:** Each tenant has its own data model, permissions, and jargon. An agent that can “Send an email” for Tenant A may need entirely different data access for Tenant. It’s essential to load a *tenant-specific context* before the agent runs: including workflows, RAG indices, feature flags, and branding. This <mark>tenant-aware</mark> context turns the agent from a generic model into “an expert in Tenant X’s world.”
    
* **Workflow Variability:** Real customers have diverse processes. One user’s data entry workflow might be 3 steps long, another’s 6 steps, and still another’s custom process with extra approvals. The agent’s planning logic must accommodate this fragmentation. You may encode workflow definitions per tenant and ensure the agent queries the right path.
    
* **Variable Data Quality:** Customer data can range from pristine to chaotic. Tenant A might have well-structured **CRM** fields; Tenant B may have years of unstructured notes. Agents must handle both gracefully, using RAG or fallback strategies for dirty data[.](https://ahmetonurslmz.medium.com/architecting-agentic-ai-for-multi-tenant-saas-5dc58d3bc3fa#:~:text=3)
    
* **Cost & Performance per Tenant:** If every tenant’s agents fire off **LLM** calls on schedules or high-frequency events, token costs and compute can skyrocket. You need tenant-scoped rate limits, batching, and caching. Otherwise, one heavy tenant can exhaust model quotas or trigger costly scaling for all.
    
* **Observability & Debugging:** When an agent misbehaves, you must pinpoint which tenant, conversation step, and tool call caused it. This demands *tenant-scoped* logs, traces, and prompt histories. Without per-tenant telemetry (traces that include tenant **IDs**, workflow state, etc.), diagnosing failures is nearly impossible.
    

To address these, adopt **tenant-aware** design patterns. Always prefix requests with a Tenant Gateway that authenticates and loads the tenant ID. The ***Context Builder*** should pull in tenant-specific configuration (settings, workflows, vector namespaces, permissions) before running the agent. Use isolated **RAG** pipelines per tenant: each has its own vector index and document ingestion, tagged with metadata to prevent cross-tenant leakage. Design tools to be tenant-aware: every tool checks the tenant **ID**, enforces that tenant’s access rights, and writes only into that tenant’s storage. Finally, log everything with tenant context in an **Observability Layer**: capture traces, prompt versions, tool actions, and errors per tenant.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766051903470/e212c88a-dd88-412b-9f84-f02a7dabab68.jpeg align="center")

**Testing & Drift:** Because agentic flows are nondeterministic, rely on scenario-based testing rather than classic unit tests. Verify not just final outcomes but the agent’s *reasoning steps*. Track metrics like task success rate, human handoff rate, latency, and per-tenant token usage . Agents will also encounter **data drift**: tenants will change their data and processes over time. Implement monitoring to detect shifts (e.g. drop in retrieval accuracy) and periodic retraining or prompt updates. Use defaults (fallback rules) for unconfigured behavior, but log such cases for later review.

**Observability & Metrics:** Treat every agent interaction as a transaction to be measured. Instrument traces and logs at each node of the workflow, including prompt and response payloads (where safe). Maintain dashboards for cost metrics (API calls, model token usage) per tenant. Regularly run red-team or chaos drills on the agent flows to ensure guardrails and fallbacks work as intended. Good telemetry enabled one team to make debugging “10× easier” by having prompt versioning and replay mode.

### **Final Conclusion:**

This article was inspired by the architecture and vision behind [**Elimu.ai**](https://github.com/Josaphat12-tech/Elimu-ai), with the code samples presented here intentionally sourced from the **official documentations of the respective technologies** (Next.js, Clerk, Supabase, [Vapi.ai](http://Vapi.ai), shadcn/ui, and Sentry) to clearly illustrate the core concepts without reproducing the full production code. The purpose was to explain *how* these tools work together to build a production-ready, agentic AI SaaS rather than to inline the entire implementation. For complete, real-world code, project structure, and deeper engineering decisions, readers are strongly encouraged to explore the full repository on [Elimu.ai GitHub](https://github.com/Josaphat12-tech/Elimu-ai) repository. This project is what inspired the writing of this article, and it serves as a practical reference for engineers looking to move from AI experimentation to building scalable, real-world AI products.