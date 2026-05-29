# 🧠 Memory Offloading Protocol
## Long-Term Token Savings for Extended Sessions

When a coding session extends across multiple hours or days, the conversational history fills up the token context window. This causes token limits to be hit rapidly and significantly slows down inference time.

To solve this, Antigravity implements **Memory Offloading**, shifting state from RAM (Context Window) to Disk (Local Files).

### When to Trigger
The AI must trigger this protocol automatically if:
1. It detects that the conversation has become extremely long (e.g., >30 turns).
2. The user explicitly requests to save progress (`/nexus-save`).
3. A major milestone or feature is completed, and the context of *how* it was built is no longer needed.

### The Protocol (3 Steps)

#### 1. Compile the State
The AI creates or updates a file named `.nexus-state.md` at the root of the project.
This file must contain:
- **Current Objective:** What is the overarching goal?
- **Completed Work:** Bullet points of what was done today (be concise, no code blocks).
- **Current Blockers/Bugs:** What is failing right now?
- **Next Immediate Steps:** The exact task to pick up next.
- **Critical Context:** Any specific architectural decisions or variables the next AI session must know.

#### 2. Clear the Cache (User Action)
After creating `.nexus-state.md`, the AI outputs a specific alert to the user:
> [!TIP]
> **Context Window Full - Optimization Recommended**
> He guardado el estado actual de nuestro trabajo en `.nexus-state.md`. 
> Para ahorrar tokens y acelerar mis respuestas, por favor **inicia una nueva sesión/chat** en Antigravity y pídeme que "retome el trabajo desde nexus-state".

#### 3. State Hydration (Next Session)
When the user starts a new session and asks to resume, the AI reads `.nexus-state.md` ONCE and immediately resumes execution from the "Next Immediate Steps".

### Rules for .nexus-state.md
- Keep it under 100 lines.
- Do not dump source code into the state file.
- Update it iteratively, removing solved blockers.
- Add `.nexus-state.md` to `.gitignore` to prevent polluting the repository.
