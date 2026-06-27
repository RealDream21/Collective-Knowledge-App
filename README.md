# Collective Knowledge Platform

README status: partial, slightly uncertain, and not the final version.

This repository contains a DAW-style ASP.NET Core MVC project for a small
collective knowledge / forum-like platform. The application is centered around
categories, topics, comments, reactions, and users with different roles.

Some details below are intentionally left open because the project still looks
like a coursework / prototype app rather than a finished product.

## General Idea

The application is meant to be a place where users can browse categories, open
topics, add comments, and react to content. Moderators and administrators have
extra rights for managing content and users.

At a high level, the project tries to cover:

- user registration and login
- category browsing
- topic creation inside categories
- comments on topics
- basic like / dislike reactions
- search through topics and comments
- sorting topics in several ways
- simple pagination
- role-based permissions
- administrator user management

The exact product direction is still not fully documented. It could be treated
as a forum, internal knowledge base, discussion board, or a learning project for
ASP.NET MVC and Identity.

## Tech Stack

- ASP.NET Core MVC
- .NET 6
- Entity Framework Core
- SQL Server / LocalDB
- ASP.NET Core Identity
- Razor views
- Bootstrap and jQuery assets from the default ASP.NET template

The project uses server-rendered pages. There is no separate frontend app.

## Repository Layout

```text
.
+-- CollectiveKnowledgePlatform/
|   +-- CollectiveKnowledgePlatform.sln
|   +-- CollectiveKnowledgePlatform/
|       +-- Areas/Identity/
|       +-- Controllers/
|       +-- Data/
|       +-- Migrations/
|       +-- Models/
|       +-- Views/
|       +-- wwwroot/
|       +-- Program.cs
|       +-- appsettings.json
+-- poze/
+-- README.md
```

The `poze` folder contains screenshots / diagrams used for presentation or
documentation, but the exact order and meaning of every image still needs to be
described properly.

## Main Domain Objects

The main entities appear to be:

- `ApplicationUser` - Identity user extended with app relationships
- `Category` - grouping area for topics
- `Topic` - main discussion / knowledge item
- `Comment` - comment attached to a topic
- `TopicLike` - reaction attached to a topic, where `1` means like and `-1`
  means dislike

The current data model is simple and mostly direct. Some behavior is implemented
inside controllers and views rather than separated into services.

## Roles

The seeded roles are:

- `Administrator`
- `Moderator`
- `User`

The rough permission model:

- anonymous users can see some category/topic pages
- authenticated users can add topics, comments, and reactions
- moderators can manage categories and some topic details
- administrators can manage users and have broader edit/delete access

This is only a practical summary from the current code. The final access-control
rules should still be reviewed because some actions combine `[Authorize]` and
`[AllowAnonymous]` in ways that may not express the intended policy clearly.

## Seeded Test Accounts

When the database is empty, the application seeds a few test users:

```text
admin@test.com           / Admin1!
moderator@test.com       / Moderator1!
user@test.com            / User1!
user_neconfirmat@test.com / User2!
```

These accounts are convenient for local testing. They should not be used as-is
in any deployed environment.

## Features, More Or Less

### Categories

Categories are the first page of the app by default. They are listed
alphabetically. Moderators and administrators can add, edit, and delete
categories.

Each category can contain multiple topics.

### Topics

Topics belong to categories and have a title and text body. Topic listing
supports:

- pagination
- search
- sorting by id
- sorting by like score
- sorting alphabetically

The current page size appears to be `3` topics per page.

### Comments

Users can add comments to topics. Comments store their content, date, user, and
topic relationship.

There is an edit view for comments, but the full comment workflow should still
be checked before calling it complete.

### Likes and Dislikes

Topics can receive likes and dislikes. A like stores `Type = 1`; a dislike
stores `Type = -1`.

The current UI tries to keep one reaction state per user per topic, but this
area may need more validation if the app is extended.

### Search

Search checks topic titles, topic text, and comment content. Results are scoped
to the selected category.

### Users

Administrators can list users, inspect a user, edit user data, change roles, and
delete users.

The delete behavior removes some related content first, but this should be
reviewed carefully before production use.

## Running Locally

Requirements, probably:

- .NET 6 SDK
- SQL Server LocalDB or another SQL Server-compatible database
- Visual Studio or the `dotnet` CLI

From the solution folder:

```powershell
cd CollectiveKnowledgePlatform
dotnet restore
dotnet build
dotnet ef database update --project .\CollectiveKnowledgePlatform
dotnet run --project .\CollectiveKnowledgePlatform
```

The default connection string is currently:

```text
Data Source=(localdb)\mssqllocaldb;Initial Catalog=CollectiveKnowledgeDB;Integrated Security=True;Multiple Active Result Sets=True
```

If LocalDB is not installed, this part needs to be changed.

## Routes

The default route points to:

```text
Categories / Index
```

There is also a custom-ish route for topic sorting:

```text
Topics / Index / {id?} / {sortOrder?}
```

This routing section is not complete. Some URLs are still easier to discover
from the views and controllers than from this README.

## Screenshots / Documentation Images

The folder `poze/` includes:

```text
diagrama (1).png
DONE.png
poza1.png
poza2.png
poza3.png
poza4.png
poza5.png
poza6.png
poza7.png
```

They are probably project screenshots and a diagram, but the README does not
yet map each image to a specific workflow.

## Known Loose Ends

This project still needs a clearer description of:

- exact business goal
- complete setup steps for a fresh machine
- database migration expectations
- deployment process
- screenshots with captions
- complete permissions matrix
- testing approach
- known bugs
- final presentation notes

Some code comments and messages are in Romanian, while this README is mostly in
English. That can be normalized later depending on the audience.

## Possible Future Work

- clean up authorization attributes and confirm public/private pages
- add validation around reactions
- add tests around permissions and delete flows
- document all screenshots
- add a proper architecture diagram
- remove committed local Visual Studio files if they are not meant to be kept
- decide whether this is a forum, Q&A app, or knowledge-base app
- improve README with exact demo flow

## Short Version

It is a .NET 6 MVC app for collective knowledge sharing with categories, topics,
comments, likes/dislikes, Identity login, and roles.

It runs locally with SQL Server LocalDB if the database is available and the EF
migrations are applied.

This README is intentionally not the final source of truth yet.
