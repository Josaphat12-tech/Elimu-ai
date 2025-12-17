<div align="center">
  <br />
    <a href="https://www.youtube.com/watch?v=XUkNR-JfHwo" target="_blank">
      <img src="public/readme/hero.png" alt="Project Banner">
    </a>
  <br />

  <div>
    <img src="https://img.shields.io/badge/-Next.JS-black?style=for-the-badge&logoColor=white&logo=nextdotjs&color=black" alt="next.js" />
    <img src="https://img.shields.io/badge/-Vapi-black?style=for-the-badge&logoColor=white&logo=vapi.com&color=green" alt="next.js" />
    <div>
      <img src="https://img.shields.io/badge/-Next.JS-black?style=for-the-badge&logoColor=white&logo=nextdotjs&color=black" alt="next.js" />
      <img src="https://img.shields.io/badge/-Vapi-black?style=for-the-badge&logoColor=white&logo=vapi.com&color=green" alt="next.js" />
      <img src="https://img.shields.io/badge/-Tailwind-00BCFF?style=for-the-badge&logo=tailwind-css&logoColor=white" />
    </div>
    <img src="https://img.shields.io/badge/-Tailwind-00BCFF?style=for-the-badge&logo=tailwind-css&logoColor=white" />
  </div>

</div>

## 📋 <a name="table">Table of Contents</a>

1. 🤖 [Introduction](#introduction)
2. ⚙️ [Tech Stack](#tech-stack)
3. 🔋 [Features](#features)
4. 🤸 [Quick Start](#quick-start)
5. 🔗 [Assets](#links)
6. 🚀 [More](#more)



## <a name="introduction">🤖 Introduction</a>

Create an LMS SaaS app from scratch featuring user authentication, subscriptions, and payments using Next.js, Supabase, and Stripe! You'll build and deploy a real-time teaching platform with Vapi, integrate an AI vocal agent, and deliver seamless, interactive learning sessions.

<div align="center">
  <a href="https://github.com/josaphat12-tech/Elimu-ai" target="_blank">
    <img src="https://raw.githubusercontent.com/josaphat12-tech/Elimu-ai/main/public/readme/thumbnail.png" alt="Elimu.ai Thumbnail" width="100%">
  </a>
</div>

## <a name="tech-stack">⚙️ Tech Stack</a>

- **[Clerk](https://clerk.com)** is a unified platform for authentication, user management, and billing. It offers embeddable UI components, flexible APIs, and admin dashboards for secure user management. Clerk also simplifies subscription management, allowing you to define plans, create pricing pages, and control access based on subscription tiers—all in one solution.

* **[Next.js](https://nextjs.org/)** is a powerful React framework that enables the development of fast, scalable web applications with features like server-side rendering, static site generation, and API routes for building full-stack applications.

* **[Sentry](https://sentry.io)** is an error tracking and performance monitoring tool that helps developers fix bugs faster by providing real-time alerts, stack traces, and performance insights.

* **[shadcn/ui](https://ui.shadcn.com/)** is a customizable component library built on Radix UI and Tailwind CSS. It offers a modern, accessible design system with pre-built components that are easy to theme and extend, making it ideal for building polished UIs with minimal effort.

- **[Supabase](https://supabase.com/)** is an open-source backend-as-a-service platform that provides instant APIs, real-time subscriptions, authentication, storage, and a PostgreSQL database, enabling developers to build scalable and secure applications with ease.

* **[Tailwind CSS](https://tailwindcss.com/)** is a utility-first CSS framework that allows developers to design custom user interfaces by applying low-level utility classes directly in HTML, streamlining the design process.
* **[TypeScript](https://www.typescriptlang.org/)** is a superset of JavaScript that adds static typing, providing better tooling, code quality, and error detection for developers, making it ideal for building large-scale applications.

- **[Vapi](https://vapi.ai)** is a developer-centric voice AI platform that enables the creation of conversational voice agents with low-latency voice interactions, speech-to-text, and text-to-speech capabilities. It supports multilingual conversations, customizable voices, and seamless integration with various AI models and tools.

* **[Zod](https://zod.dev/)** is a TypeScript-first schema validation library that provides a simple and expressive way to define and validate data structures. Zod ensures data integrity by catching errors early during development.

## <a name="features">🔋 Features</a>

👉 **AI Voice Agents**: Take tutoring sessions with voiced AIs specializing in the topics you want to get better at.

👉 **Authentication**: Secure user sign-up and sign-in with Clerk; Google authentication and many more.

👉 **Billing & Subscriptions**: Easily manage plans, upgrades, and payment details.

👉 **Bookmarks and Session History**: Let users organise their learning by bookmarking tutors and accessing previous sessions.

👉 **Code Reusability**: Leverage reusable components and a modular codebase for efficient development.

👉 **Create a Tutor**: Create your own AI tutors, choosing a subject, topic, and style of conversation.

👉 **Cross-Device Compatibility**: Fully responsive design that works seamlessly across all devices.

👉 **Database Integration**: Uses Supabase for real-time data handling and storage needs.

👉 **Modern UI/UX**: Clean, responsive design built with Tailwind CSS and shadcn/ui for a sleek user experience.

👉 **Scalable Tech Stack**: Built with Next.js for a fast, production-ready web application that scales seamlessly.

👉 **Search Functionality**: Find tutors quickly with robust filters and search bar.

and many more, including code architecture and reusability.

## <a name="quick-start">🤸 Quick Start</a>

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)

**Cloning the Repository**

```bash
git clone https://github.com/josaphat12-tech/Elimu-ai.git
cd Elimu-ai
```

**Installation**

Install the project dependencies using npm:

```bash
npm install
```

**Set Up Environment Variables**

Create a new file named `.env` in the root of your project and add the following content:

```env
# Sentry
SENTRY_AUTH_TOKEN=

# Vapi
NEXT_PUBLIC_VAPI_WEB_TOKEN=

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_IN_FALLBACK_REDIRECT_URL=/
NEXT_PUBLIC_CLERK_SIGN_UP_FALLBACK_REDIRECT_URL=/

# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

Replace the placeholder values with your actual ImageKit, NeonDB, Upstash, and Resend credentials. You can obtain these credentials by signing up on: [Supabase](https://supabase.com/dashboard), [Clerk](https://clerk.com), [Sentry](https://sentry.io), [Vapi](https://vapi.ai).

**Running the Project**

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the project.

## <a name="links">🔗 Assets</a>

Assets and snippets used in the project can be found in the **video kit** included in the repository.

<a href="https://github.com/josaphat12-tech/Elimu-ai" target="_blank">
  <img src="public/readme/videokit.jpg" alt="Video Kit Banner">
</a>

## <a name="more">🚀 More</a>

**Advance your skills with Next.js Pro Course**

Enjoyed creating this project? All me on all social media platforms for a richer learning adventure and also feel free to walk through my blogs where I post tutorials on to make your own projects and trust me you will find more detailed explanations, cool features, and exercises to boost your skills. Give it a go!

**Owner & Contact**

This project is maintained by **Bitingo Josaphat JB**.

- 💼 LinkedIn: [linkedin.com/in/bitingojosaphatjb/](https://linkedin.com/in/bitingojosaphatjb/)
- 🐙 GitHub: [github.com/josaphat12-tech](https://github.com/josaphat12-tech)
- 📝 Blog: [bitingo-the-deep-neural-nets.hashnode.dev/](https://bitingo-the-deep-neural-nets.hashnode.dev/)
- 📧 Email: [bitingojosaphat@gmail.com](mailto:bitingojosaphat@gmail.com)

## 👥 Contributors

We'd like to thank our contributors who have helped make this project better:

- 🌟 [Eszter Focze](https://github.com/Focze-Eszter)

<div align="center">
  <a href="https://github.com/josaphat12-tech/Elimu-ai" target="_blank">
    <img src="https://raw.githubusercontent.com/josaphat12-tech/Elimu-ai/main/public/readme/image_.png" alt="Project Banner" width="100%">
  </a>
</div>
