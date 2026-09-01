# MLEbotics: Robotics + AI Automation Platform

**Live site:** https://mlebotics.com

A multi-tenant platform for deploying, monitoring, and orchestrating physical
robots and AI agents from a single dashboard — built to grow from a solo-dev
project into a scalable cloud product, one phase at a time.

This repository documents the project. The application source is closed;
see [Why no source code?](#why-no-source-code) below.

## Overview

MLEbotics is a Turborepo/pnpm monorepo covering the full stack of a
multi-tenant SaaS: a public marketing site, an operator console, a visual
workflow editor, a tRPC API layer, and an early robotics integration layer
(ROS2/MQTT/RTSP bridges) for connecting physical hardware to the platform.

The platform also incubates and sells standalone products through the site
— **ChromaShift**, a system-wide color-accessibility overlay for colorblind
users, is one such product, developed and shipped via this platform and
sold as its own closed-source app.

## Apps & modules

| App | Stack | Description |
|---|---|---|
| `apps/marketing` | Astro 4 + Tailwind | Public marketing site (mlebotics.com) |
| `apps/console` | Next.js 15 + tRPC + Tailwind | Operator dashboard — robots, workflows, automation |
| `apps/studio` | Next.js 15 + Tailwind | Visual workflow/world editor |
| `apps/docs` | Markdown | Platform documentation |
| `platform/` | TypeScript | Core engine interfaces — world/entity, automation/workflow, plugin system |
| `robotics/` | TypeScript | Agent runtime + ROS2/MQTT/RTSP hardware bridge skeletons |
| `infra/db` | Prisma | Organization/User/Membership/Role schema |

## Tech stack

- **Frontend:** Next.js 15, Astro 4, Tailwind CSS
- **API:** tRPC v11
- **Data:** Prisma (relational DB)
- **Monorepo tooling:** Turborepo, pnpm workspaces
- **Robotics integration:** ROS2, MQTT, RTSP bridge adapters

## Why no source code?

MLEbotics incubates and sells commercial products (like ChromaShift) through
the same platform, so the full monorepo — including product source that
was previously public — has moved to a private repository to protect what's
sold. This showcase exists so the live site and product case studies can
still be evaluated without exposing source others could otherwise build
from for free. Contact me for a walkthrough if you'd like to see more.

## Status

Live and actively developed as of 2026. Phases 1–4 (monorepo/UI shells,
identity + API layer, platform engine interfaces, robotics agent layer)
complete; Phase 5 (autonomy engine, marketplace, enterprise docs) planned.
