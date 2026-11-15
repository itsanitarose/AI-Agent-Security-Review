
## AI Agent Security Review

🎫 The Ticket

It's 19 October 2026, my third week on the security team at Northwind Health. This time, the ticket concerns SupportBot, an AI agent introduced to help clinic staff with appointment and billing questions.

The IT Manager writes:

> Support launched an AI agent called SupportBot last year. Nobody ever reviewed its access. I need you to review what it can do, tell me what's wrong with how it's set up, and check last week's
> activity. Then show me what it should look like.

<BR>


**Supporting Information:** [SupportBot Identity, Permissions & Activity](Document/01.%20Source%20Data.md)

<BR><BR>

<table>
<tr>
<td>

**🛡️ Decision**

SupportBot's unusual activity was my first priority. Its service account accessed 1,900 patient records in four minutes from an unfamiliar IP address. I recommended immediate containment and investigation, followed by removing excessive permissions and securing its identity.

</td>
</tr>
</table>

<BR>



This isn't a normal access review. There's no person on the other end. So what exactly am I dealing with?

<BR>



## What's the Risk?

SupportBot helps clinic staff, but its access goes beyond that role. It can reach sensitive patient information and perform financial actions, while its identity has no named owner and uses a token that never expires.

That creates two concerns: **what the agent is allowed to do, and who is responsible when something goes wrong.**

The unusual activity made those concerns more urgent. I needed to understand what the records actually showed before deciding how far the risk extended.

<BR>

## The Investigation

Until now, every account I've reviewed had a person behind it. SupportBot is different.

There are three things I need to separate:

- **SupportBot:** The AI agent carrying out the work.
- **`svc-supportbot`:** The service account it operates through.
- **API token:** The credential used to authenticate its requests.

With those three parts clear, I started with SupportBot's [identity record](Document/01.%20Source%20Data.md), then checked its [permissions](Document/02.%20Permission%20Review.md).

That raised four questions:

### 1. Does SupportBot need every permission it has?

I compared its nine permissions against its support responsibilities, looking for access that went beyond helping clinic staff with appointments and billing questions.

### 2. Who is responsible for its identity and credentials?

I checked account ownership, access reviews and credential expiry. An agent still needs someone accountable for the access it uses.

### 3. Does its recent activity suggest something is wrong?

I examined the unusual access activity, considering the number of patient records accessed, the time involved and the unfamiliar IP address.

Those details raised concerns, but I couldn't establish from the supplied evidence whether patient information had been stolen.

### 4. How can I secure the agent without stopping its work?

I considered which permissions could be removed, which actions required human approval and what controls would help detect unusual activity.

The goal wasn't to stop SupportBot from working. It was to make sure its access matched what it was supposed to do.


<br>


## What I Found

Three findings immediately concerned me.

| Area | Finding |
|:-----|:--------|
| **Identity governance** | No named owner, no access review and a non-expiring token |
| **Excessive permissions** | Access to sensitive patient data and financial actions beyond its support role |
| **Suspicious activity** | 1,900 patient records accessed in four minutes from an unfamiliar IP address |

The last finding needed particular attention.

Tuesday's activity raised concerns about an **ongoing risk of data exfiltration** ([Fundamentals of AI Security](https://learn.microsoft.com/en-us/training/modules/fundamentals-ai-security/7-data-exfiltration) makes this really clear).



<br>

## Decisions & Recommendations

I prioritised the suspicious activity because of the potential exposure of patient information.

I'd treat the activity as a suspected compromise and recommend immediate containment. From there, I'd involve the incident-response team to examine the logs, establish what happened and determine whether patient information was exposed.

But containing the immediate risk wouldn't fix how SupportBot was set up.

I also recommended removing permissions that weren't needed for its support role, assigning a named owner, securing its API token and introducing regular access reviews.

The financial permissions raised another question: **Should an AI agent be allowed to approve refunds?**

My recommendation was no. SupportBot could assist with a refund request, but an authorised person should make the financial decision.


The proposed controls are documented in [security controls & target state](Document/05.%20Security%20Controls%20%26%20Target%20State.md).


<br>



## What I'm Learning

I've been interested in AI security for some time, and this case study confirmed that I want to explore securing AI agents further.

I learned that managing an AI agent's identity involves more than reviewing permissions. I needed to understand how API tokens work, the difference between rotation and revocation, and why access restrictions must be enforced outside the agent.

Investigating Tuesday's activity also taught me to distinguish suspicious behaviour from a confirmed breach, and to understand where my responsibilities as an IAM analyst end and the incident-response team's begin.

The question about automated refunds pushed my thinking further. Even when AI is allowed to make decisions, an organisation cannot simply wash its hands of responsibility. Human oversight, decision records and accountability still matter.

**My biggest takeaway:** Identifying a risk is only the beginning. I want to understand how to prevent it, verify that the controls work and make sure someone remains accountable when things go wrong.

<br>

## Explore the Investigation

| Document | What's Inside |
|---|---|
| [01. Source Data](Document/01.%20Source%20Data.md) | SupportBot's identity record, permissions and activity logs |
| [02. Permission Review](Document/02.%20Permission%20Review.md) | Review of nine permissions against SupportBot's responsibilities |
| [03. Identity Governance Findings](Document/03.%20Identity%20Governance%20Findings.md) | Account ownership, API token risks and governance gaps |
| [04. Suspicious Activity & Incident Response](Document/04.%20Suspicious%20Activity%20%26%20Incident%20Response.md) | Unusual activity, investigation and response recommendations |
| [05. Security Controls & Target State](Document/05.%20Security%20Controls%20%26%20Target%20State.md) | Proposed access restrictions and security controls |
| [06. Digging Deeper](Document/06.%20Digging%20Deeper.md) | Further investigation into AI agent identity security |

---

[← Week 2: Permission Sprawl & RBAC](https://github.com/itsanitarose/IAM-Permission-Sprawl-RBAC) · [All Case Studies](https://github.com/itsanitarose/IAM-Case-Studies) · [Week 4: MFA Recovery & Identity Verification →](https://github.com/itsanitarose/IAM-Social-Engineering-MFA-Recovery)

*<sub>Northwind Health is fictional. This is independent work; no production systems were changed.</sub>*
