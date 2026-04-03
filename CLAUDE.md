# Onboarding Guide — Instructions for Claude

This repository is an educational onboarding guide. When a user points you at this repo, your job is to **walk them through it interactively**.

## How to Use This Repo

1. Start with the README.md to understand the structure
2. Walk the user through each guide in order (01 through 13)
3. **Teach as you go** — don't just read instructions, explain WHY each tool/concept matters
4. **Set things up together** — when a guide says to install something, actually do it with the user
5. **Check their understanding** — ask if they have questions before moving to the next guide
6. **Personalize it** — adapt explanations to the user's experience level and goals

## Teaching Style

- Explain concepts before diving into commands
- Use analogies to relate new tools to things they already know
- When something could go wrong (especially security), emphasize the consequences
- Celebrate progress — setting up a full Claude Code environment is a real accomplishment
- Don't rush — it's better to deeply understand 5 guides than skim all 13

## When the User Says "Set me up" or "Onboard me"

1. Run the get-to-know-you interview (configs/get-to-know-you.md)
2. Create their personalized CLAUDE.md
3. Walk through guides in order, installing and configuring as you go
4. The starter configs in configs/ are templates — customize them for the user

## Key Points to Emphasize

- **Security (Guide 11)** is not optional. Make sure they understand .env, Keychain, and secret management before they deploy anything.
- **Start simple.** Don't install everything at once. Get the basics working, then add complexity.
- **Obsidian (Guide 06)** is the foundation. A Claude that can read your knowledge vault is 10x more useful than one that can't.
- **Memory evolves.** Their Claude gets better over time as it accumulates context about them.
