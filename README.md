# Skills Registry

Our internal skill registry. Stores references to both ecosystem skills and skills we author.

## Structure

```
skills/
├── registry/
│   ├── skills-lock.json   # Master index (auto-generated)
│   ├── skill-security-auditor.yaml  # local: our security scanner
│   ├── ag2.yaml           # ecosystem
│   ├── superpowers.yaml    # ecosystem
│   └── ...                # 30 more ecosystem entries
└── source/
    └── skill-security-auditor/  # local skill implementation
```

## Adding a New Skill

**Ecosystem skill:** Add a YAML entry to `registry/<name>.yaml` and update `skills-lock.json`.
**Local skill:** Copy to `source/<name>/`, add YAML to `registry/`, update `skills-lock.json`.

## Remote

https://github.com/meowju/skills — default branch: `main`

## License

Individual skills have their own licenses. See each skill's `LICENSE.txt` or `LICENSE` file.