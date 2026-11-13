# Book Skills — One Book, One Skill, Explained Simply

Each week I take one IT/cybersecurity book, turn its core ideas into a **Claude Agent Skill** (`skills/<book>/SKILL.md`), and publish short **"explain like I'm 5"** videos in **English and Arabic**.

كل أسبوع آخذ كتاب في التقنية أو الأمن السيبراني، أحوّل أفكاره الأساسية إلى **مهارة Claude**، وأنشر فيديوهات قصيرة **بالعربي والإنجليزي** تشرحه بأبسط طريقة.

## How to use a skill

```bash
git clone https://github.com/Imanmalek/skills.git
cp -R skills/skills/linux-basics-for-hackers ~/.claude/skills/
```

Then in Claude Code ask things like *"explain Linux permissions like I'm 5"*. In Claude.ai, zip the skill folder and upload it under **Settings → Capabilities → Skills**.

## Skills

| # | Skill | Level | Topic |
|---|---|---|---|
| 1 | [linux-basics-for-hackers](skills/linux-basics-for-hackers/SKILL.md) | Beginner | Linux for security |

## Notice

Skills contain original summaries and explanations written in my own words, with credit to the source book. No book text, images or files are reproduced or distributed. Buy the books to support the authors.

All content is educational. Practice only on systems you own or on legal training labs.

## License

[MIT](LICENSE)
