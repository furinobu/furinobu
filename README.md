## Intro
Hi, I'm Harry! I am a student in the Faculty of Information Engineering at the Chinese University of Hong Kong. I love learning computer science, video game programming, and anything about a computer.

## 🤓 My values
🍏 Beginner's mindset and curiosity<br>
💨 High capability and adaptability<br>

## 🌱 Goals
- Make indie games
- Learn more hardware knowledge

## 🧠 Languages that I know and use
### Web
- HTML5
- CSS3
- Javascript
### Software
- Python
- C/C++
- Rust
- Java
- Pascal

## 💡 Projects
- Minesweeper in Pascal
- Telegram/discord bots
- Elevator Simulator
- A Monopoly-like game written in Rust with Godot engine


**Coding with command blocks is awesome:**

![Minecraft command block programming is in another level](https://i.kym-cdn.com/photos/images/original/001/340/291/07b.jpg)


<!--
## 🔗 Get in touch
- Personal site: 
- Dev.to: 
- StackOverflow: 



- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

## Portfolio development

The portfolio at the repository root is a SvelteKit static site. The contact form posts JSON to `${PUBLIC_BACKEND_URL}/api/contact`.

Set both required public build-time values in the tracked root `.env` before starting, building, or deploying; a blank or missing value fails the frontend build. The root `.env` is used by the GitHub Pages build. The contact backend is maintained in the separate private `furinobu/portfolio-backend` repository; its `.env` stays private on the VPS.

```sh
PUBLIC_BACKEND_URL=https://api.example.com \
PUBLIC_TURNSTILE_SITE_KEY=your-site-key \
npm run build
```

The GitHub Pages workflow reads the tracked root `.env` directly. Do not expose the server-side Turnstile secret in the static site.

```sh
npm install
npm run dev
npm run build
```

For a project GitHub Pages deployment, build with its repository path:

```sh
BASE_PATH=/your-repository-name npm run build
```

The generated static site is in `build/`. `static/.nojekyll` is included for direct GitHub Pages uploads.
