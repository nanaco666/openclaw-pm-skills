# TapDB Connector setup

## Audience

Only for teams that already use TapDB and have valid MCP keys.

## Preconditions

- TapDB project access
- Correct region key (`TAPDB_MCP_KEY_CN` or `TAPDB_MCP_KEY_SG`)
- Python 3

## Minimal setup

1. Put the key in shell config
2. If needed, set `SSL_CERT_FILE`
3. Validate with `list_projects`
4. Only then run source / income / retention / lifecycle queries

## Common pitfalls

1. Wrong region key → project list appears empty
2. macOS Python SSL verification failure → fix with `SSL_CERT_FILE`
3. User asks for unsupported metrics → explain nearest available metric
4. User pastes secret into chat → do not repeat it back in full
5. Project ID guessed from memory → wrong; always confirm from `list_projects`
