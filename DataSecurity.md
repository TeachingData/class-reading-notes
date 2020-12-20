# Data Protection, Regulation, and Privacy concerns
#### as related to Databases and Data Engineering

*This is a small overview: a full Information Security course would be needed for full coverage*

## Sections:
1. [Data Protection vs. Data Privacy](#data-protection-vs-data-privacy)
2. [Regulations effecting Data](#regulations-effecting-data)
    - [License and Copywrite vs. Regulation](#license-and-copywrite-vs-regulation)  
3. [Types of vulnerabilities and issues](#types-of-vulnerabilities-and-issues)
4. [Protection methods (as related to Intro to Data Engineering)](#protection-methods)

## Data Protection vs Data Privacy

Though these are ***industry terms with definitions which vary by focus or area***, <ins>for the purpose of these sections - they can be thought of as</ins>:

- Data Privacy = Legal and Ethical issues surrounding defining what data is inherently private and who should have access to it (and under what circumstances they should)

- Data Protection = Technical issues, to include software, hardware, and policy based issues, surrounding preventing unauthorized access to data.

Though these two issues are intertwined (and should be studied by all in IT/IS):

**Managers (who focus on policy issues) and Engineers (focused on the technical)** tend to focus on Data Protection based on the industry and other regulations. **Lawyers** typically handle full Data Privacy concerns as most require interpretation (or enforcement) of laws, regulations, or the creation of these. As such the following sections contain <ins>some information on Data Privacy but the overall focus is much more about Data Protection</ins>.

## Regulations effecting Data

There are various regulations which effect data and the definition of what is private information vs. protected vs. public. Which and what you need to know is nearly completely dependent on what region your company will operate in (think China's definition of *"private"* vs. US definition), what type of data you will be handling, and what type of organization you are. However, there are some broad regulations which all IT/IS students should be aware of:

- Privacy Act of 1974 
- Children’s Online Privacy Protection Act 
- FERPA (Family Educational Rights and Privacy Act) 
- HIPPA (Health Insurance Portability and Accountability Act of 1996)
- General Data Protection Regulation (GDPR – Europe) 

#### Privacy Act of 1974

This created the idea of Personal Identifiable Information (PII) and was the first real regulation regarding how it should be handled.

#### [Children's Online Privacy Protection Act](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)

Ever been to a site or signed up for a service (like Gmail or Stackoverflow or Reddit or ...) which required you to confirm your age before you could use it?

All of this stems from this act. So the main take away from this (at a basic level and in most industry) is:
- **Don't allow people under the age of 13 to signup for your website** so you don't fall under this
  - A simple "I am over the age of ..." checkbox is usually fine
  - If collecting payments: most just have person use a credit card to verify identity and checkbox
- If you have to (like Nick Jr.'s website): then avoid collecting any PII from these users
  - Ensure only PII is collected from parents and you get parental conscent before collecting
- If you **must** collect PII from <13 - consult a lawyer

Basically, if you have a startup or project which is website or has public signup (accounts): you might consider adding that *checkbox* into your signup process.

Oddly enough - this is <ins>not why sites ask if you are over the age of 18</ins>. That has to do with State's and Countries' rules on *"age of consent"*. As, if a website has you sign a contract for a special position or permissions, like [moderator positions](https://meta.stackexchange.com/questions/357379/questions-about-the-new-minimum-age-for-diamond-moderators) or github private repositories, you need to be at the *"age of consent"* for the contract to be binding or they have to get parental/guardian permission) and mature content issues.

### FERPA

### HIPPA

### GDPR (General Data Protection Regulation)
