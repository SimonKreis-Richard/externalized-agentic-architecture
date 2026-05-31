# Variables — Single Source of Truth

> All {VAR} references in skills resolve from this file.
> When a value changes, modify this file ONLY. Skills read at runtime.

## Agent Identity
- AGENT_NAME: [your-agent-name]
- USER_NAME: [your-name]

## Paths
- HERMES_HOME: ~/.hermes
- SKILLS_DIR: ~/.hermes/0-skills-curated
- SINGLE_SOURCE_OF_TRUTH: ~/.hermes/0-hermes-single-source-of-truth
- USER_DATA_DIR: ~/.hermes/0-user-data
- SOUL_PATH: ~/.hermes/SOUL.md
- OUTPUT_DIR: ~/Downloads
- WIN_HOME: /mnt/c/Users/[WINDOWS_USERNAME]

## Models
- PRO_MODEL: [your-provider/your-pro-model]
- FLASH_MODEL: [your-provider/your-flash-model]
- PROVIDER: [your-provider-name]

## API Keys (set in .env, not here)
- TAVILY_API_KEY: see .env
- EXA_API_KEY: see .env
- JINA_API_KEY: see .env
