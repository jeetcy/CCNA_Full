# 📓 My Troubleshooting Log

Every time you break and fix something, add a row. Interviewers love these stories!

| Date | Lab | Symptom | How I found it (commands) | Root cause | Fix |
|---|---|---|---|---|---|
| 2026-11-17 | Example | PC1 can't reach PC3 | `show interfaces trunk` → VLAN 10 missing from the allowed list | Trunk allowed list was wrong | `switchport trunk allowed vlan add 10` |
| | | | | | |
