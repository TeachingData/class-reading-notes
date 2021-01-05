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

#### Privacy Act of 1974

This created the idea of Personal Identifiable Information (PII) and was the first real regulation regarding how it should be handled. It was the start that nearly everything else (in the US) was built from and [contains three rights](https://www.ssa.gov/privacy/privacy_act_1974.html):

  1. the right to request their records, subject to Privacy Act exemptions
  2. the right to request a change to their records that are not accurate, relevant, timely or complete
  3. the right to be protected against unwarranted invasion of their privacy resulting from the collection, maintenance, use, and disclosure of their PII.

All database systems (when properly designed) should have effective methods for handling all three of these issues but the first 2 explicitly. 

#### [Children's Online Privacy Protection Act](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)

Ever been to a site or signed up for a service (like Gmail or Stackoverflow or Reddit or ...) which required you to confirm your age before you could use it?

All of this stems from this act. So the main take away from this (at a basic level and in most industry) is:
  - **Don't allow people under the age of 13 to signup for your website** so you don't fall under this
    - A simple "I am over the age of ..." checkbox is usually fine
    - If collecting payments: most just have person use a credit card to verify identity with the above checkbox
  - If you have to (like Nick Jr.'s website): then avoid collecting any PII from these users
    - Ensure only PII is collected from parents and you get parental/guardian conscent before collecting
  - If you **must** collect PII from <13 - consult a lawyer

Basically, if you have a startup or project which is website or has public signup (accounts): you might consider adding that *checkbox* into your signup process.

*A Note on 13+ vs. 18+*: Despite the name, the Children's Act is <ins>not why sites ask if you are over the age of 18</ins> it only focuses on information collection of children. The 18+ checkboxes have to do with State's and Countries' rules on *"age of consent"*. These consent regulations (and others) dictate issues such as mature content viewership and the enforability of contracts signed for special positions or permissions. Looking at examples such as a website which offers [moderator positions](https://meta.stackexchange.com/questions/357379/questions-about-the-new-minimum-age-for-diamond-moderators): the site may require signees to be at the *"age of consent"* (which varies by state but is generally 18) for the special position as the agreement for that position needs to be binding but only 14 to use the site itself.

### [FERPA](https://www2.ed.gov/policy/gen/guid/fpco/ferpa/students.html)

This really only applies to us as this regulation dictates what is considered private information in regards to students. For the most part, it also won't apply to your tasks this year but it can have some *"grey"* areas. A major one being sharing photos and videos of students performing activities at school. To take an example directly from [the Department of Education](https://studentprivacy.ed.gov/faq/faqs-photos-and-videos-under-ferpa)

> If a school maintains a close-up photo of two or three students playing basketball with a general view of student spectators in the background, 
> the photo is directly related to the basketball players because they are the focus of the photo, but it is not directly related to the students 
> pictured in the background. Schools often designate photos or videos of students participating in public events (e.g., sporting events, concerts,
> theater performances, etc.) as directory information and/or obtain consent from the parents or eligible students to publicly disclose photos or
> videos from these events.

A database field which shows if consent is given (boolean) and a seperate binary field (pdf) which has a copy of the signed consent is usually all that is needed for situations like the above but it is something any site or project which deals with student data needs to consider. 

### HIPPA

The Health Insurance Portability and Accountability Act or HIPPA provides the regulations on health related information (in both physical and electronic forms). If you end up building systems for hospitals, insurance companies, or other health related fields: the importance of this regulation is obvious. However, it also affects a number of information systems in somewhat surprising ways.

For instance, one compay I worked for handled loan payments for individuals and part of the system including storing information on deferrment of payments (pausing one's loan payments) which included <ins>hardship due to health related issues</ins>. This meant that this simple banking app, fell under HIPPA because it needed to store some minor records (a file or two) that proved the health issue. A second example would be nearly every HR system I've setup which included <ins>reasons behind sick days or extended leaves of absence</ins> - one of which was part of a lawsuit due to a supervisor entering in a worker's longterm disability into a public record instead of the more secure and confidential file it was suppose to be stored in.

If it is health related, it will probably need to be *"HIPPA Compliant"*.

## Next header

## Next header
