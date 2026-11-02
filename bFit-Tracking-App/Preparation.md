# Preparation: bFit

As of November 1, 2026. Planned; no application code is available yet.

## Goal

Develop a personal fitness web app for workouts, exercises, and progress tracking. It serves as a first, manageable application to guide the process from code through CI to persistent operation in a home lab.

## Selection

| Area | Recommendation | Why? |
| --- | --- | --- |
| Language | C# / .NET 10 | To leverage existing knowledge; the focus is on the web app and its operation. |
| UI | Blazor Web App with Interactive Server | Browser access on smartphones and desktops; no separate native client initially. |
| Backend | ASP.NET Core within the same application | Separate business services; no additional API just for the application’s own server-side UI. |
| Data | PostgreSQL with EF Core 10 and Npgsql 10 | Get familiar with relational data and an open-source database. |
| Authentication | ASP.NET Core Identity with cookies | Suitable for the browser-based server-side application. |
| Architecture | A web project with clear responsibilities | A single image and database simplify the initial setup. |

Npgsql documents the [EF Core Provider 10](https://www.npgsql.org/efcore/release-notes/10.0.html). Ensure the provider and EF Core are set to compatible major versions. Microsoft explains [the version requirements](https://learn.microsoft.com/en-us/ef/core/providers/). Separating data access, business logic, and the UI does not initially require microservices.

Interactive Server requires an active connection; see [Blazor Render Modes](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/render-modes?view=aspnetcore-10.0). This works well for private access via VPN. Offline operation or a focus on mobile use would be a reason to reevaluate the UI choices.

## Initial Scope

- Login; each user can only view and edit their own data.
- Exercise catalog with names and optional descriptions.
- Workout with date, exercises, sets, reps, and weight.
- History and simple progress overview.
- Input validation for missing values and invalid numbers.

The model can start with `Exercise`, `Workout`, `WorkoutExercise`, and `WorkoutSet`. Verify user assignment on the server side and store weights using a specified unit.

## Acceptance and Operation

1. A saved session persists after the app and database are restarted.
2. A user cannot view or modify other users’ sessions.
3. Changes to a session save the associated data consistently.
4. A guide launches the app and database from a fresh checkout.
5. CI tests an image; Argo CD later deploys it with the home lab configuration.
6. A database backup was restored to a fresh instance, and the training session was verified.

Start PostgreSQL locally via Compose. In a home lab, the setup includes persistent storage, backup, and restore instructions. Database high availability requires its own operating model.

## Future Expansion

Training plans, personal bests, CSV export, charts, and reminders. Add a native MAUI client or a separate API if device capabilities or the need for an additional client warrant it.

## Planned Repo Structure

```text
bFit/
├── README.md
├── Preparation.md
├── src/BFit.Web/
├── tests/
├── deploy/base/
├── compose.yaml
└── .gitlab-ci.yml
```

## Description for the Future README

> bFit is a planned fitness web app built with C# and Blazor. 
It is designed to store workout sessions and progress in PostgreSQL. 
The focus is on a clear data model, user-based access control, and reliable operation with backup and restore capabilities. 
The first milestone is a workout history that can be accessed locally.


