# Developer Cookbook — api-oss-devtools
**Stack:** Python 3.11, Click, rich, pyinstrument, AIOSS_FORMAT
**Domain:** Sovereign developer tooling: CLI, debugger, profiler for Anticloud development
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```bash
# Inspect a project
anticloud inspect TIER_4/K_SGLANG --show-aioss --show-benchmarks

# Profile an inference run
anticloud profile --module PAX_INFERENCE_CORE --duration 30s --output ./profile.html

# Debug with PAX assistance
anticloud debug --traceback ./error.log --pax-model ./pax-27b-q4.gguf
# PAX output: "Root cause: AIOSS chain file locked by another process. Fix: aioss unlock ./chain.aioss"

# AIOSS chain inspector
anticloud aioss inspect ./audit.aioss --last 10 --verify
# Entry #42: SHA3-256=8b4a8a4f... VALID | PAX_INFERENCE_CORE | 2026-10-01T12:34:56Z

# Scaffold new project
anticloud scaffold --tier TIER_4 --name K_NEWPROJECT --template inference_agent
```

```python
from api_oss_devtools import DevProfiler
with DevProfiler(aioss_chain="./devtools.aioss") as prof:
    result = pax.infer(prompt)
prof.print_report()  # rich table: function, calls, time, %
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every api-oss-devtools output:
chain_hash = aioss_append("./api_oss_devtools.aioss",
                           result_bytes, "api-oss-devtools")
```

## Performance & Integration

pyinstrument statistical profiler: <2% overhead. rich for terminal UI (no curses). PAX debugging context: pass last 50 log lines + stack trace. Integration: reads AIOSS chains from all projects, invokes ANTICODE_AGENT for fix suggestions, feeds profiles to api-oss-analytics.
