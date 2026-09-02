## What Is Scope?

Scope is your boundary. It defines what you protect, what you modify, and what you ignore.

Without scope, you try to fix every failure in the ecosystem, and you end up shipping unstable, unmaintainable code.

---

## Why Scope Is Essential

When you try to fix problems outside your Reach, you risk multiple things such as:

- **Creating More Issues:** Modifying code or behavior you don't fully own or understand.
- **Liability:** Creating fragile local patches for bugs that belong to an external vendor or upstream dependency.
- **Wasting Resources:** Solving problems that other teams or vendors are responsible for maintaining.

Your scope keeps your execution CLEAN and your system stable.

---

## How to Determine Scope

Your scope is defined by your boundary and the Platform you work in for Example:

If You Work In:

**Windows Kernel Then your Scope is:** Windows platform code and native OS subsystems | Defensive Drivers or Mitigations, APIs used to Mitigate, and recovery paths for third party drivers. |
**Linux Kernel Then your Scope is:** Core OS drivers, subsystems, and native interfaces | Patch contributions, etc etc. |
**Application LayerThen your Scope is:** Your code base, internal & reserved modules, and build scripts | Workarounds for OnGoing OS or third-party bugs. |

---

## What to Do When You Find an Issue

Ask yourself these three questions:

### 1. Do I own this codebase?

- **Yes** → Fix it. It is inside your scope.
- **No** → Do not attempt to patch their source code or binary. (You don't even know the code, what will you patch?)

### 2. If I don't own it, does it AFFECT my system's stability?

- **Yes** → (e.g., a buggy third party kernel driver): The third party code itself is out of scope for a fix or Patch, but mitigating it (e.g., adding driver That Influences it Directly, or recovery tools to Get rid of it) is inside your platform scope (Unless if it is Not Mandatory and it is Uncontrollable and may break if you attempted mitigation).
- **No** → (e.g., a bug in a non critical external app): Report it to the owner. It is entirely outside your scope.

### 3. Finally question your life Choices.

---

## Personal Example

I mitigate Windows issues whenever possible.

But if the issue is in a third-party driver (e.g., NVIDIA), I don't touch it. Why should I?

- It's not mandatory for core functionality.
- I don't have the source code.
- Trying to fix it could break something I don't even know exists.

**It is third-party, not mandatory, and outside my control. So I leave it alone.**

And this doesn't just get applied for NVIDIA only it applies for lets say AV / EDR issues.

---

## Conclusion

**Stay where you can keep your focus.** Build something that actually works not just random software.

Scope protects you from wasting time, breaking things, and solving problems that aren't yours to solve.




> Scope was made because some people had been contacting me about the recent 0 days so i think i may had made myself clear
