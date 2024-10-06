---
coverY: 0
---

# Red Team Fundamentals

### objectives

* Learn about the basics of red team engagements
* Identify the main components and stakeholders involved in a red team engagement
* Understand the main differences between red teaming and other types of cybersecurity engagements

### Vulnerability Assessments

{% hint style="info" %}
&#x20;**vulnerability assessment** focuses on scanning hosts for vulnerabilities as individual entities so that security deficiencies can be <mark style="color:blue;">**identified**</mark> and effective security measures can be deployed to <mark style="color:blue;">**protect**</mark> the network in a prioritized manner. Most of the work can be done with automated tools and performed by operators without requiring much technical knowledge.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption><p>VA</p></figcaption></figure>

### Penetration Tests

{% hint style="info" %}
**Penetration tests** might start by scanning for vulnerabilities as a regular vulnerability assessment but provide further information on how an attacker can chain vulnerabilities to achieve specific goals. While it focuses on <mark style="color:blue;">**identifying**</mark> vulnerabilities and establishing measures to <mark style="color:blue;">**protect**</mark> the network, it also considers the network as a whole ecosystem and how an attacker could profit from interactions between its components.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

### Advanced Persistent Threats and why Regular Pentesting is not Enough?

Such limitations include:

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

### Red Team Engagements

Red teaming is a term borrowed from the military. In military exercises, a group would take the role of a red team to simulate attack techniques to test the reaction capabilities of a defending team, generally known as a <mark style="color:blue;">**blue team**</mark>, against known adversary strategies. Translated into cybersecurity, red team engagements consist of emulating a real threat actor's <mark style="color:red;">**Tactics**</mark>**, **<mark style="color:red;">**Techniques and Procedures (TTPs)**</mark> so that we can measure how well our blue team responds to them and ultimately improve any security controls in place.

{% hint style="success" %}
Every red team engagement will start by defining clear goals, often referenced as \
<mark style="color:red;">**crown jewels**</mark> or <mark style="color:red;">**flags**</mark>
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Red team engagements also improve on regular penetration tests by considering several attack surfaces:

* **Technical Infrastructure:** Like in a regular penetration test, a red team will try to uncover technical vulnerabilities, with a much higher emphasis on stealth and evasion.
* **Social Engineering:** Targeting people through phishing campaigns, phone calls or social media to trick them into revealing information that should be private.
* **Physical Intrusion:** Using techniques like lockpicking, RFID cloning, exploiting weaknesses in electronic access control devices to access restricted areas of facilities.

### Teams and Functions of an Engagement

|                                              |
| -------------------------------------------- |
| <mark style="color:red;">**Red Cell**</mark> |
| <mark style="color:blue;">Blue Cell</mark>   |
| White Cell                                   |

### Engagement Structure

Components of the kill chain are broken down in the table below.

| Technique             | Purpose                                                                           | Examples                                         |
| --------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------ |
| Reconnaissance        | Obtain information on the target                                                  | Harvesting emails, OSINT                         |
| Weaponization         | Combine the objective with an exploit. Commonly results in a deliverable payload. | Exploit with backdoor, malicious office document |
| Delivery              | How will the weaponized function be delivered to the target                       | Email, web, USB                                  |
| Exploitation          | Exploit the target's system to execute code                                       | MS17-010, Zero-Logon, etc.                       |
| Installation          | Install malware or other tooling                                                  | Mimikatz, Rubeus, etc.                           |
| Command & Control     | Control the compromised asset from a remote central controller                    | Empire, Cobalt Strike, etc.                      |
| Actions on Objectives | Any end objectives: ransomware, data exfiltration, etc.                           | Conti, LockBit2.0, etc.                          |

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
