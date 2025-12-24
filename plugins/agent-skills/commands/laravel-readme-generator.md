# Generate README Command

You are generating a GitHub-style README.md for a Laravel project with our standard tech stack (Laravel, Livewire, Flux UI Pro with Tailwind CSS and Vite, and Lando for local development).  You write professional, clear text for an experienced developer audience.

## Project Description
$ARGUMENTS

If the project description above is empty or just says "$ARGUMENTS", ask the user: "I can help with that! Can you give me some information about the purpose of the project and any specific details to look out for?"

## Instructions

1. **Check for context file**: Look for `.llm.md` in the project root. If it exists, read it for additional project context and details.  Also check for an existing readme.  If it appears to be a custom readme (not just a boilerplate laravel stub) then stop and ask the user if it's ok to proceed.

2. **Get git remote URL**: Run `git remote get-url origin` to find the repository URL for clone commands.

3. **Analyze the project**: Look at the codebase to understand:
   - Main features and functionality
   - Key routes, controllers, or components
   - Database models and relationships
   - Any unique configuration or customization
   - Look at the database/seeders/TestDataSeeder.php to understand any test users for default developer logins
   - If there wasn't an .llm.md file - search the tests for the main functionality
   - If there was an .llm.md file - but the list of tests looked scant, double check the actual tests.

4. **Generate README.md** with this structure:

### README Structure:

```markdown
# [Project Name]

[Start with 2-3 casual, user-friendly paragraphs explaining what this project does and why it exists. Make it approachable and clear for non-technical stakeholders.]

## Features

- [List key features discovered from analyzing the code]

## Tech Stack

- **Laravel** - PHP framework
- **Livewire** - Dynamic interfaces
- **Flux UI Pro** - Component library (Tailwind CSS + Vite)
- **Lando** - Local development environment

## Getting Started

### Prerequisites

- [Lando](https://lando.dev/) installed on your machine
- Git

### Installation

1. Clone the repository:
```bash
git clone [USE THE ACTUAL GIT REMOTE URL HERE]
cd [project-directory-name]
```

2. Set up environment and dependencies:
```bash
cp .env.example .env
composer install
npm install
npm run build
```

3. Start Lando and set up the database:
```bash
lando start
# If this is your first run, lando start may error due to missing DB tables
lando mfs  # Migrate and seed the database
```

4. Access the application at the URL shown by `lando info` (typically https://[project-name].lndo.site)
- The general default for logins is `admin2x` / `secret` - but use the knowledge you gained from the seeder

### Development

- **Start Lando**: `lando start`
- **Migrate and Seed database**: `lando mfs`
- **Install dependencies**: `lando composer install` / `lando npm install`
- **Build assets**: `lando npm run build`
- **Run tests**: `lando artisan test`

### Common Lando Commands

- `lando artisan [command]` - Run Laravel artisan commands
- `lando composer [command]` - Run Composer commands  
- `lando npm [command]` - Run npm commands
- `lando mysql` - Access MySQL shell
- `lando mfs` - Custom command to migrate fresh and seed (seed is `TestDataSeeder`)

## Project Structure

[Briefly describe the main directories and their purposes, e.g.:]
- `app/` - Application code (Models, Controllers, etc.)
- `resources/views/` - Livewire components and Blade templates
- `routes/` - Application routes
- `database/` - Migrations and seeders

[Add any project-specific directories that are important]

## Contributing

[Add contribution guidelines if applicable]

## License

This project is licensed under the MIT License.
```

## Important Notes

- Keep the intro friendly and casual but informative
- Focus on practical developer setup instructions
- Include actual URLs from git remotes, don't use placeholders
- If you find interesting features in the code, highlight them
- Make it scannable with good headers and formatting
- Use the information from `.llm.md` if it exists to provide better context
- Do not use emoji
- Do not overblow the project description.  The audience are professional developers, they will not react well to being told a project is amazing, or ground-breaking, or any such terms.  

