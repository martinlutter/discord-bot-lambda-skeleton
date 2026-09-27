---
name: setup-new-bot
description: Turn a fresh clone of the discord-bot-lambda-skeleton into a new, named bot project.
disable-model-invocation: true
argument-hint: <bot-name>
---

# Set up a new bot from the skeleton

Bot name: $ARGUMENTS. If empty, ask for it. Use kebab-case for the package name and AWS profile,
PascalCase for the stack name.

1. Rename:
   - `package.json`: `name`, `description`, and `AWS_PROFILE=discord-bot` in `scripts.deploy`
   - `app.config.ts`: `botName`
   - `README.md`: title and the intro paragraph; replace the `discord-bot` profile name in the setup steps.
     Drop the "Rename the project" section.
   - `.claude/skills/setup-new-bot/`: delete it once setup is done; it only makes sense for the skeleton.
2. Git history: if `git log` still shows the skeleton's history, ask before running `rm -rf .git && git init`.
3. `cp .env.dist .env` if `.env` is missing. Walk the user through README "Create the Discord application" for the values.
   Don't invent values, and don't print the token back.
4. Check `~/.aws/config` for the profile. If it's missing, tell the user to run `aws configure --profile <name>`.
   Never write credentials yourself.
5. Run `npm install && npm run build && npm test && npm run lint && npx cdk synth`.
6. Commit the rename.
7. List the remaining manual steps, in order:
   1. Invite the bot to the server (README step 5)
   2. `npm run deploy:commands`
   3. `npx cdk bootstrap --profile <name>` (once per account/region)
   4. `npm run deploy`
   5. Paste the `InteractionsEndpointUrl` output into Developer Portal → General Information → Interactions Endpoint URL
   6. Try `/ping`
