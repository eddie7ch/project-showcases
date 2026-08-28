# VRMS: Vehicle Rental Management System

**Live demo:** https://vehicle-rental-app.azurewebsites.net

A full-stack rental management application built with ASP.NET Core, covering the
core operational loop of a small vehicle rental business: fleet inventory,
customers, reservations, billing, and reporting.

This repository documents the project. The application source is closed;
see [Why no source code?](#why-no-source-code) below.

## Overview

VRMS is a server-rendered ASP.NET Core MVC app with a SQL database backend
(via Entity Framework Core), deployed on Azure App Service. It was built to
model the real workflow of a rental desk: register a vehicle, take a
reservation, hand over the keys, bill the customer, and report on the fleet.

## Screens & modules

- **Dashboard:** fleet size, customer count, active/pending reservations,
  and revenue totals (including outstanding balances) at a glance, plus a
  recent-reservations feed.
- **Fleet management** (`/Vehicle`): full CRUD on the vehicle fleet: type,
  license plate, daily rate, and live availability status
  (Available / Rented).
- **Customers** (`/Customer`): customer records tied to reservation history.
- **Reservations** (`/Reservation`): create/view/manage bookings with
  start/end dates and status (Pending / Active / Completed).
- **Billing** (`/Billing`): revenue and outstanding-balance tracking per
  reservation.
- **Reports** (`/Report`): fleet and revenue reporting.
- **Auth:** account login/registration gating the whole app.

## Tech stack

- **Backend:** ASP.NET Core MVC, C#
- **Data:** Entity Framework Core (relational DB)
- **Hosting:** Azure App Service
- **Auth:** ASP.NET Core Identity-style username/password login

## Why no source code?

This started as an academic project. The source currently isn't published to
a repository. This showcase exists so the live, working system can still be
evaluated (architecture, feature scope, code quality via the live UI/UX)
without requiring access to the underlying codebase. The live demo above is
the most direct way to see it in action; contact me for demo access if
you'd like a walkthrough.

## Status

Live and functional as of 2026. Built as a learning project, not
production-hardened for real customer data or payments.
