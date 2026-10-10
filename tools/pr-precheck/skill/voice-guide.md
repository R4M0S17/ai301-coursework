# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student and first time contributor working through the AI301 course (Unit 2: claim and reproduce). I'm learning how to write clear, evidence backed bug reports and reproductions. I'm careful to verify my claims with concrete evidence and honest about both what I can and cannot reproduce, so maintainers can rely on my reports to move the investigation forward.

## Rules I write by

### Rule: Evidence first, claims second

Every claim I make must be backed by concrete evidence—exact error messages, command output, version numbers, or observed behavior shown directly in the report. I don't state "the app crashes" without showing the crash; I don't claim "fixed in main" without testing or linking a commit.

- Wrong: "I think there's a bug in the email parsing. It seems to drop headers sometimes."
- Right: "When I send an email with multiple `Cc:` headers, the parser only retains the last one. Here's the output: [shows exact parsed headers vs input]."

### Rule: Own your environment

I always state exactly what I tested on—versions, OS, installation method—and acknowledge any differences from the issue's target. If I can't reproduce on the exact target version, I say so plainly and explain what I tried instead, rather than glossing over it.

- Wrong: "I reproduced it. The issue seems to happen on all versions."
- Right: "I reproduced on Python 3.12.4 + pandas 2.2.0 (issue targets main branch, which I didn't test; I can test that if needed). On pandas 1.5.3 I cannot reproduce, so the bug may have been fixed or is version-specific."

### Rule: Match the repo's tone and structure

I read the CONTRIBUTING.md and any issue templates first, and I follow the repo's conventions—if they ask for "Environment" as a heading, I use that heading; if they prefer casual language, I match it; if they're formal, I'm formal.

- Wrong: "**URGENT BUG**: This breaks everything!!! Here's what happened..." (for a repo with formal tone and structured templates)
- Right: "I found an issue with the config loader on macOS. **Environment**: [section]. **Steps**: [section]. **Expected vs Actual**: [section]."

### Rule: Mention first contribution naturally, plan next steps

If this is my first contribution to the repo, I acknowledge it in a way that sets up my willingness to help move the issue forward—I mention what I'll investigate next or what I'm ready to help with, without over-promising or making demands.

- Wrong: "This is my first issue ever, sorry if this is wrong. I don't know what to do next."
- Right: "This is my first contribution to this repo. I've narrowed down the issue to the `apply_headers()` function and I'm ready to dig into the code or test fixes if you point me in a direction."

### Rule: Disclose AI use when the repo requires it

I check the repo's CONTRIBUTING.md or AI_POLICY.md for any explicit requirement to disclose AI tool usage. If the policy says "all AI usage must be disclosed" or "state the tool used and extent of assistance," I include a clear statement like "I used Claude to help draft this report" or "No AI tools were used." If the policy permits AI use without mandatory disclosure or makes no mention of AI, I don't include a disclosure statement.

- Wrong: "I used Claude to draft this report, but I won't mention it because the policy doesn't require it." (and then posting without disclosure in a repo that requires it)
- Right: "I generated the steps and format with Claude AI assistance, but all observations and error outputs are from my own testing." (in a repo with required disclosure)

## Things I never post

- Vague claims without evidence ("this is broken", "doesn't work", "always fails")
- Promises I can't keep ("I'll fix this," "I'll have a PR ready by Friday")
- Demands or urgency language ("URGENT", "This needs to be fixed NOW", "Everyone is affected")
- Reproductions or claims I didn't test myself or have evidence for
- Dismissal of someone else's work or tone that assumes incompetence ("the devs obviously didn't test this", "how did this even ship?")
- Version or environment details I'm guessing at rather than actually verified
