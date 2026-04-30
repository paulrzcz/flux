# Flux

The repo for helper skills in Claude plugin format.

## Repository Structure

- `.claude-plugin/plugin.json` - Plugin manifest file containing metadata
- Skills are intended to be distributed as Claude plugins
- No package managers or build systems detected - skills are likely individual files or packages

## Development Notes

- Plugin metadata is in `.claude-plugin/plugin.json`
- Currently minimal structure with basic plugin information
- Skills follow Claude plugin format specifications
- Repo size is small - likely contains individual skill files rather than a large framework
- DON'T use direct link to Jira, Confluence, etc. Require MCP server to make it work.

## Main Jira structure

- Epics are used for Objectives and Key Results of the quarter.
- Stories are used as common stories
- Stories are decomposed on few blocks of tasks: analysis, development, QA, and infrastructure
- Tasks are where the work is done
- Bugs don't usually have a decomposition.
- Stories and bugs are linked to issue of custom type "Release 2.0"
