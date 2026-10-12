# Configuration Management Tools: CCNA Study Notes

Organized study note on **configuration management tools (Ansible, Puppet, Chef)**, part of the CCNA network automation and programmability topics.

## Files

| File | Description |
|---|---|
| `Configuration_Management_Tools.docx` | The formatted study note (Word document) |
| `README_Configuration_Management.md` | This file: overview and how the note is organized |

## What the note covers

1. **Overview.** Why configuration management tools matter, especially in large networks.
2. **Configuration Drift.** How individual changes make devices deviate from the company standard.
3. **Configuration Management Without Automation.** Saving configs as text files in a shared folder, and the flaws of that approach.
4. **Configuration Provisioning.** Applying changes (including to new devices), why one-by-one SSH does not scale, and the two essential components: templates and variables.
5. **Configuration Management Tools.** Origin of the tools and the tasks they perform.
6. **Ansible.** Agentless, push model, SSH, Python. Playbooks, inventory, templates, variables.
7. **Puppet.** Agent-based, pull model, TCP 8140, Ruby. Manifests and templates.
8. **Chef.** Agent-based, pull model, TCP 10002, Ruby-based DSL. Resources, recipes, cookbooks, run list.
9. **Quick Comparison.** Side-by-side table of the three tools.
10. **Summary.** One-line recap of each tool.

## Key points to remember

- **Configuration drift** = a device's config deviates from the company standard because of individual changes. Avoid it.
- **Templates and variables** are essential components in all three tools.
- **Ansible**: agentless, **push** model over SSH, written in Python. Playbooks and variables in YAML, templates in Jinja2.
- **Puppet**: agent-based, **pull** model, TCP **8140**, written in Ruby. Server is the Puppet master. Uses manifests.
- **Chef**: agent-based, **pull** model, TCP **10002**, Ruby-based DSL. Uses resources, recipes, cookbooks, and run lists. Least popular of the three.
- Not all Cisco devices support Puppet or Chef agents.

## Formatting conventions

- Numbered sections with sub-sections (6.1, 6.2, ...)
- Bullet points for explanations
- Yellow-shaded **KEY RULE** boxes for the most important rules
- Diagrams embedded where they are explained (templates + variables, Ansible push model, Puppet/Chef pull model)
- Comparison table for the three tools

## Related notes

- REST API Authentication
- REST APIs (SDN and northbound interfaces, CRUD and HTTP verbs, HTTP request vs response)
- Routing Fundamentals
- Static Routes
