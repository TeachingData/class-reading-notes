# Data Protection, Regulation, and Privacy concerns
#### as related to Databases and Data Engineering

*This is a small overview: a full Information Security course would be needed for full coverage*

## Sections:
1. [Data Protection vs. Data Privacy](#data-protection-vs-data-privacy)
2. [Regulations effecting Data](#regulations-effecting-data)
3. [Types of vulnerabilities and issues](#types-of-vulnerabilities-and-issues)
4. [Protecting against](#protecting-against)

## Data Protection vs Data Privacy

Though these are ***industry terms with definitions which vary by focus or area***, <ins>for the purpose of these sections - they can be thought of as</ins>:

- Data Privacy = Legal and Ethical issues surrounding defining what data is inherently private and who should have access to it (and under what circumstances they should)

- Data Protection = Technical issues, to include software, hardware, and policy based issues, surrounding preventing unauthorized access to data.

Though these two issues are intertwined (and should be studied by all in IT/IS):

**Managers (who focus on policy issues) and Engineers (focused on the technical)** tend to focus on Data Protection based on the industry and other regulations. **Lawyers** typically handle full Data Privacy concerns as most require interpretation (or enforcement) of laws, regulations, or the creation of these. As such the following sections contain <ins>some information on Data Privacy but the overall focus is much more about Data Protection</ins>.

## Regulations effecting Data

There are various regulations which effect data and the definition of what is private information vs. protected vs. public. Which and what you need to know is nearly completely dependent on what region your company will operate in (think China's definition of *"private"* vs. US definition), what type of data you will be handling, and what type of organization you are. However, there are some broad regulations which all IT/IS students should be aware of:

- Privacy Act of 1974 
- FERPA (Family Educational Rights and Privacy Act) 
- HIPPA (Health Insurance Portability and Accountability Act of 1996)

#### Privacy Act of 1974

This created the idea of Personal Identifiable Information (PII) and was the first real regulation regarding how it should be handled. It was the start that nearly everything else (in the US) was built from and [contains three rights](https://www.ssa.gov/privacy/privacy_act_1974.html):

  1. the right to request their records, subject to Privacy Act exemptions
  2. the right to request a change to their records that are not accurate, relevant, timely or complete
  3. the right to be protected against unwarranted invasion of their privacy resulting from the collection, maintenance, use, and disclosure of their PII.

All database systems (when properly designed) should have effective methods for handling all three of these issues but the first 2 explicitly. 

### [FERPA](https://www2.ed.gov/policy/gen/guid/fpco/ferpa/students.html)

This really only applies to us as this regulation dictates what is considered private information in regards to students. For the most part, it also won't apply to your tasks this year but it can have some *"grey"* areas. A major one being sharing photos and videos of students performing activities at school. To take an example directly from [the Department of Education](https://studentprivacy.ed.gov/faq/faqs-photos-and-videos-under-ferpa)

> If a school maintains a close-up photo of two or three students playing basketball with a general view of student spectators in the background, 
> the photo is directly related to the basketball players because they are the focus of the photo, but it is not directly related to the students 
> pictured in the background. Schools often designate photos or videos of students participating in public events (e.g., sporting events, concerts,
> theater performances, etc.) as directory information and/or obtain consent from the parents or eligible students to publicly disclose photos or
> videos from these events.

A database field which shows if consent is given (boolean) and a seperate binary field (pdf) which has a copy of the signed consent is usually all that is needed for situations like the above but it is something any site or project which deals with student data needs to consider. 

### HIPPA

The Health Insurance Portability and Accountability Act or HIPPA provides the regulations on health related information (in both physical and electronic forms). If you end up building systems for hospitals, insurance companies, or other health related fields: the importance of this regulation is obvious. However, it also affects a number of  information systems in somewhat surprising ways.

For instance, one compay I worked for handled loan payments for individuals and part of the system including storing information on deferrment of payments (pausing one's loan payments) which included <ins>hardship due to health related issues</ins>. This meant that this simple banking app, fell under HIPPA because it needed to store some minor records (a file or two) that proved the health issue. A second example would be nearly every HR system I've setup which included <ins>reasons behind sick days or extended leaves of absence</ins> - one of which was part of a lawsuit due to a supervisor entering in a worker's longterm disability into a public record instead of the more secure and confidential file it was suppose to be stored in.

If it is health related, it will probably need to be *"HIPPA Compliant"*.

## Types of vulnerabilities and issues

    1. Injection
    2. Broken Authentication
    3. “Sensitive Data Exposure”

We will look at one of these and one other issue not listed.

#### Injection
These are not an all inclusive list, for instance it doesn't meantion ***social engineering*** issues. However, it **must** be noted that **Injection has been the number 1 vulnerability for more than 5 years**. One should learn lessons and improve software design year to year but legacy software and programmers [failing to sanitize their inputs](https://www.explainxkcd.com/wiki/index.php/327:_Exploits_of_a_Mom) have been a continuing thorn in the side of data protection and look to remain a trend. Though it is not the only step in preventing this, it is the first: [Always use prepared statements](https://bobby-tables.com/).

#### Social Engineering
This tactic tries to manipulate or deceive a user or employee in order to gain entry into a computer system or steal personal or private information. Though the *"social"* points to its main usage (through social media including simple email) the full field of social engineering employees full psychological manipulation to trick users into making mistakes or giving away too much information.

Social engineering attacks run the gamet from mass phishing attacks to fully invested teams investigating possible victims to gather background information, both as a business (finding weak security protocols) and as individual (using people's history to find "ins"), before attempting an attack. Usually using this information in multiple attacks which build on each other to find holes to exploit before taking full control or gaining full access to information.

Social Engineering includes (not exhaustive list):
    1. **Phishing**: See any college's manditory training on "phishing". I will only note that *phishing* does not mean email. SMS messages (text) and Social Media attacks (discord, twitter, etc) are used for phishing as well with **text and phone calls seeing an exponential increase this last year**.
    2. **Tailgating**: A person with no authentication will follow an employee into a secured area. This is everything from impersonating a delivery driver to gain access to a floor to just running up and having employee hold open the door *because its polite*.
    3. **Scareware**: The sunsetting of IE has seen more of these this year. Malicious javascript which full screens and locks a browser open (making it look like your computer is "frozen") and plays audio or displays text telling you to *"call us to unlock your device"* is one of the most common. But any software exploit which *makes it look like* your locked out is a form of this (in the last example restarting or killing the process would "fix" the problem).
    4. **Ransomware**: The worse form of above, typically involves someone calling the number given in the scareware example and then allowing someone to remote-in with full access. It is always something that locks a user or group of users (in a business) out of their device(s) - **the best defense for this is training, proper firewalls (lock out external access), and proper backups (to recover after & use as a comparison to determine data loss)**

## Protecting Against

The data backups and automation for preventing ransomware (or any unwanted access) are the realm of data engineering. As it is typically the network admin, data engineers, and devops who build Machine Learning based bots that:

    1. Scan access records to see what IPs are accessing the system and determine if any are not allowed or unknown (Observe)
    2. Scan Log files to determine if anything looks out of the ordinary based on training data (the previous logs, Orient)
    3. Determines what action needs to be taken based on these differences using other training data (previous incidents with results, Decide)
    4. Either block access for IPs directly (usually suspend), Isolate the access of an IP, or report to admin using a notification (Act)

We call this an OODA loop and its a common method of building AI/ML bots for automated access and protective control systems. It is not the only form of automated protection but these can (and are) a whole course unto themselves. What needs to be stated is these should not be the only form of protection, rather one must practice a **"Defense in Depth" (DOD) strategy** which builds in multiple layers of protection and access controls. For our example:
- Using Direct Access controls to setup layers (Student access allows one to join a class, "graduate TA" or Professor access is needed to see grades)
- Ensure all developers understand the need (and how to use) prepared statements (or move to 2-tier development)
- Building logging into the development teams' policies (i.e. dev teams actually add documentation and logging as they build)
  - These should be standardized to allow for better processing later
- Use automation to scan and process Logs (AI/ML)
