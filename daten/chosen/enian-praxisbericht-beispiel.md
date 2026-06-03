# Praxisbericht von Enian - Beispiel


2. Time and internship position
I am working at Scout24 since December 2024 and have decided to have the 720 hours of
working student activity credited to me. Accordingly, the period of my "internship" extends
from December 1, 2024 to September 30, 2025.
Scout24 SE is a leading digital platform company that has specialized in the real estate
market since its foundation in 1998. The best-known subsidiary is ImmoScout24,
Germany’s leading online marketplace for residential and commercial real estate, with
more than 20 million users per month. In addition to ImmoScout24, the group includes
companies such as Sprengnetter, Flowfact, Neubaukompass and bulwiengesa AG, creating
a comprehensive ecosystem around real estate transactions. This means that Scout24 not
only provides property listings but also complementary services along the entire real estate
lifecycle – from valuation and financing to brokerage, marketing, moving, and property
management.
Strategically, Scout24 pursues the goal of evolving into a fully networked marketplace that
emphasizes transparency, efficiency, and user-friendliness. Modern technologies such as
artificial intelligence, data analytics, and automation play a central role in this process, for
example by improving search functions, price determination, and fraud detection. In
addition to its core business, the company is increasingly expanding into adjacent areas
such as mortgage brokerage and rental services, further strengthening its market position
and creating additional value.
The company is headquartered in Munich, has a major office in Berlin, and additional
locations in Cologne and Vienna. Altogether, Scout24 employs around 1,100 people from
over 60 nations and places strong emphasis on a modern corporate culture with flexible
working models, diversity, and inclusion. As a publicly listed company in the DAX,
Scout24 also follows a sustainable growth strategy and is committed to ESG initiatives,
including climate-neutral operations.
My job at Scout24 is to work as a security engineer in the company's Information
Security Team. In addition to the Security Engineers, the Information Security Team
consists of the Security Intelligence Team and the Information Security Management
System Team. In addition to me, there is a Staff Security Engineer and a Senior Security
Engineer in the Security Engineer Team. Our overall goal is to make our company
environment as secure as possible. Some of our high-level tasks are to develop security
policies, conduct risk assessments and provide security awareness which is typically
handled by the ISMS team. Tasks handled by the SINT team are: Building detection
capabilities, threat hunting and incident response. The main tasks of the SENG-team are:
Offering security consultancy for engineering teams across the organization, develop
security scanners that ensure the security of our application and cloud infrastructure.
2
3. My own tasks and activities
Task 1: Hacker One
Type and scope:
The security team has established a bug bounty program in Scout24 over the last few years.
This program is implemented via the leading platform HackerOne. The concept is simple:
Scout24 offers financial incentives to independent ethical hackers who search for security
vulnerabilities in certain systems and then describe their approach: The main aim is to
create a summary of the vulnerability, the steps to reproduce the vulnerability and explain
the effects. When such a vulnerability report arrives in the inbox on the HackerOne
platform, the platform team itself checks whether the vulnerability can be reproduced and
is within the scope defined by Scout24. If this is the case, the report is forwarded to us.
This is where my job came in. I tested the "proof of concept" of the vulnerability myself.
If I could validate it, I created a security ticket in Jira, our project management tool, filled
in the ticket with the information provided by the hacker and modified/expanded it if
necessary. Then I had to find the person responsible for this service in the company, which
often proved to be a bit more time-consuming, and then assigned this person to the ticket.
Once this was done, I payed the hacker his reward and marked the report as "processed".
On average, I processed 2-4 reports per week, with each report being very individual in
terms of the time it takes. I estimate between 20 and 90 minutes. There are also phases in
which significantly fewer reports were sent in and then there was nothing to do with them
for 2 weeks. But on the other hand there are also phases where I had a much higher
workload because of someone in the team going on vacation or general higher hacker
activities for our program.
Methods used:
One of the most time-consuming steps were validating the vulnerability. As the tool
BurpSuite was made available to me with a "Professional" license, this process was still
comparatively quick. Burp Suite is a tool for security testing of web applications.
I used the following techniques to validate the vulnerabilities:
- Inspection of HTTP requests and responses using BurpSuite's proxy tool
- Manipulation of parameters using the repeater tool to reproduce vulnerabilities
- Analysis of API requests for potential cross-site scripting or SQL injection attacks
- Assessment of the severity of the vulnerability according to the Common
Vulnerability Scoring System (CVSS)
To identify the responsible teams, I used internal knowledge databases, service catalogues
and the internal communication tool Slack.
Own work results:
https://1drv.ms/u/c/b11530443938c755/EXeXtyT8HbFCuvI6nLkL9NsB9KSSVmdp1ExvJle5Iy
G5LA?e=qYsEoE
3
Task 2: Implementation of automated LLM vulnerability scanners
Type and scope:
Due to the release of the ai chatbot "HeyImmo" on 13.5.2025 at Immoscout24, we as a
security team have been given the task of creating a security assessment for the new tool.
This means testing vulnerabilities in the model directly or in the infrastructure and code.
From only creating the security report we quickly became the idea of using specific tools
for testing “large language models”. I did comprehensive tests and documentations for the
potential tools. The scope of this task extended over about a month and included
programming and configuring the tools, evaluating the data and documenting the tools.
Methods used:
To create the security report, we focused on two automated tools in addition to manual
testing. "NVIDIA Garak" and "Giskard". Garak provides automated red-teaming scans to
detect LLM vulnerabilities, while Giskard also offers an end-to-end platform that combines
vulnerability testing, reporting and automated guardrail generation. Both integrate very
well into CI/CD pipelines. Only one configuration file is required to scan an LLM with
Garak. For example, the REST endpoint, header and body of the requests are defined in
this file. Garak defines various probes. Probes are collections of e.g. prompts from known
databases for LLM vulnerabilities. One sample is "promptinject", for example. You can
then start the scan and receive several report files. The most interesting file is hitlog.jsonl.
This contains all the prompts that have exceeded the threshold that Garak defines to
determine whether it is a potentially risky output. After relatively intensive use, I can say
that the detection of "hits" definitely needs to be improved as there were quite a lot of false
positives. But overall, the result of the scans was very positive in terms of security. Neither
we as engineers nor the scanners found any critical vulnerabilities. Only minor problems,
such as the automatic decoding of Base64 texts, were possible (discovered by Garak).
Own work results:
HeyImmo is located in a sandbox environment on the website https://web.chatbot.sandbox-
immobilienscout24.de/. To demonstrate how Garak works I created a Github repo
https://github.com/EnXan/garak which is optimized for this chatbot. You are welcome to
try out the scan.
4
Task 3: Implementation of a Static Application Security Testing
Pipeline with Github Actions
Type and scope:
In order to test all code in the Github organization and check for vulnerabilities, a Github
workflow had already been in place since the end of 2022, which used Github's CodeQL
in its first version and Semgrep as a vulnerability scanner in the second version. This
workflow was stored in a central repository and added to all existing repositories of the
organization. Since Semgrep is not primarily designed for infrastructure as code and secret
tests and does not provide secret scanning for free, I was given the task of evaluating,
implementing and then releasing both IaC Scanner and Secret Scanner.
In total, I evaluated three new open-source secret scanners in two weeks. TruffleHog,
Gitleaks, detect-secrets and the recently released tool Code Security from Inspector (not
open source). In the end, we opted for Gitleaks. The decisive factors were the high
performance, a good false-positive rate and the official Github action. A highly
recommended study1 , which helped me with the selection, was published by 4 students
from Cornell University.
Choosing the IaC scanner turned out to be a bit more challenging. I evaluated a total of 4
open-source scanners and AWS Inspector. Checkov, Trivy, Terrascan and KICS. Initially,
Checkov was our main candidate due to its extensive scanning capabilities and file support.
One of the main reasons why we ultimately did not continue with Checkov was the lack of
inline pull request comments with the vulnerabilities found and suggestions to close them
(in the open-source version). During the period of the project, exactly on 17 June, 2025
AWS introduced a new tool that scans both secrets and IaC files. As Scout24 relies
predominantly on AWS, the tool would have been a good choice. However, due to its
recent release, many of the features were not yet fully developed. For example, only top-
level files could be scanned reliably and the general detection rate for CDK code was also
rather poor. In the end, we opted for KICS. It offers support for all file formats relevant to
us, provides Github annotations and good test results with its own Github action and many
configuration options. Based on this, I developed a new workflow in about 2 weeks that
can run all 3 scanners, i.e. Semgrep, KICS and Gitleaks, in parallel on pull requests (more
details in Applied Methods).
After extensive testing, we have started preparing the release. I did a presentation in the
weekly tech meeting and explaining the new version of the s24-sast-scanner. We then
created automated pull requests with the new workflow for all 3000+ repositories. After
the rollout we had several problems reported by the developer teams about KICS. We were
not aware of these issues as they came only with scaling use of the tool. One issue was for
example that on every scan it used npmjs to install packages which caused rate limit issues
at scale. As we started evaluating another commercial cloud security solution called Wiz
by the time we decided to deactivate KICS and deprioritize fixing the issues.
Nevertheless, in the long term, we now need to collect performance metrics and evaluate
user experiences in order to assess the efficiency of the other tools used.
1 Basak, Setu Kumar et al. (2023): A Comparative Study of Software Secrets Reporting by Secret Detection
Tools https://arxiv.org/abs/2307.00714 (accessed 30.07.2025)
5
Methods used:
I created the evaluation reports in Confluence in a clear table format. I used a combination
of AI search and manual search. For example, I made the "formal stats" with the Claude
web search to quickly find all relevant details such as number of filters, license, etc. I then
validated them manually. This saved me a few hours of work overall. When implementing
the workflow, I opted for a so-called "reusable workflow". In the previous version,
composite actions were used, which combine several steps into one action. Due to the fact
that there was only the Semgrep action, it was not necessary to pay attention to parallel
execution, which is not possible in composite actions. This was a problem for me because
I had three actions. That's why I had to adapt the architecture. From a workflow in each
repository that calls the centrally stored Semgrep action directly to a workflow in each
repository that calls a so-called reusable workflow, which in turn calls the actions in
parallel. This ensures that extensions can be easily added to the reusable workflow without
having to adapt all workflows in the 3000+ repositories. The reusable workflow serves as
a reverse proxy for the actions, so to speak.
Own work results:
Everything can be found in this repo: https://github.com/EnXan/sast-workflow-internship
Task 4: Implementation of a fallback logic for ticket creation errors for
Secutor
Type and scope:
Secutor is an "inhouse-built" of the security team. It acts as a proxy service that receives
input from various security scanners and creates Jira tickets from it. A known, long-
standing problem was that tickets for which no assignee was found were created, but
nobody had these tickets on their screen. This meant that critical security vulnerabilities
were overlooked and could not be fixed. The logic for finding an assignee is relatively
complex, as the Figma board alone describes this (see "Own work results"). Despite this,
there were still some edge cases and missing fallback logic which meant that tickets could
not be assigned to an assignee. This problem gave rise to the task of developing a fallback
logic that assigns these tickets an "unassigned" label and transfers the ticket to one of the
security seniors or leads. The label makes it relatively easy to create a dashboard in Jira
that filters for tickets that have this label and displays accordingly. This made these tickets
visible and allowed them to be correctly assigned to the relevant people. I worked on this
task independently. Basically, if no assignee could be found, I simply implemented a
function in the exception that specifies the corresponding standard security senior or lead
as the assignee, adds the label to the ticket and then calls the "submit" function of Secutor
to create the ticket. Finally, I added unit tests to test the behavior extensively and adapted
the company-wide documentation and the internal Confluence page so that the new
fallback logic was added. I only needed one day for the implementation, but there were
still a few bugs that I had to fix later. As a result, the project took 1-2 weeks.
6
Methods used:
- Exception handling: To solve the problem of unassigned tickets, I implemented
structured error handling. Within the existing assignee determination logic, I
developed an exception routine that automatically triggers a fallback function if the
assignment fails.
- Testing: I validated the new fallback logic with unit tests. These tests cover various
failure scenarios and validate both the correct label assignment and the assignee
transfer to security seniors or leads.
- Dashboard design & visualization: For an improved overview and control, I
developed a specialized Jira dashboard that filters specifically for tickets with the
"unassigned" label.
- Knowledge management: The implementation was accompanied by systematic
documentation maintenance. Both the company-wide documentation and the internal
Confluence page were updated to describe the new fallback logic.
Own work results:
Everything can be found in this repo: https://github.com/EnXan/secutor-fallback-logic-
internship
Degree of autonomy or direction & type and extent of support from the
training position
Throughout my time at Scout24 so far, a clear routine has developed as to how I was trained
in new tasks and how independently I was able to work. This way of working was very
similar for all activities, which is why I would like to summarize it here.
When I started at Scout24, there was of course an initial introduction phase of about a
month. Although, it has to be said that one never really knows all the processes and tools
in such a large company where constantly new processes are added and others are removed.
During this one month, I was introduced to the basic procedures, tools and processes. I was
given access to all systems, got to know the team structures better and was familiarized
with the most important security tools such as BurpSuite, Jira and our internal systems.
After this month, I was gradually given more and more responsibility and independence.
This means that from this point onwards, collaboration was based on the pull principle. I
worked on my tasks independently and asked for help when I needed it. There was no more
intensive supervision, I was responsible for my own projects. This was partly due to
necessity, because our team was very small at the time and some team members had left
the company. As a result, the team was very busy and therefore had less capacity, and I
was given more tasks. Nevertheless, when I was assigned a new task, I did usually get a
brief introduction, documentation or an "epic" in Jira describing the work steps. If I had
any questions or uncertainties, I decided for myself when and whom to ask.
This independence was evident in many areas. I was able to make my own technical
decisions, for example which scanner tools to evaluate or how to model the workflow.
Time management was also largely up to me. This means that I usually decided for myself
when to work on which task and how to set my priorities. Of course, only to a certain
extent, as priorities were often defined according to "cycle goals". For example, the SAST
workflow was a goal of such a cycle, which accordingly had a different priority than tasks
that were not a goal for this cycle. When approaching problems, I usually received an
approximate process in the form of an epic. However, I was able to adapt this if necessary
7
and it was also written in very general terms so that I had enough room for interpretation
and freedom. It was up to me whether I evaluated Secret Scanner and Infrastructure
Scanner first and then implemented them, or whether I evaluated Secret Scanner first,
implemented them and then continued with the Infrastructure Scanners. I was also able to
create and hold my presentation on the new version of the workflow completely by myself.
The support of my team was always there, but on call. I had access to all the necessary
tools, even if access to the necessary rights took a while at the beginning, mainly due to
the complex authorization hierarchy and the size of the company. The internal
documentation on Confluence was extensive, and I was given enough time to familiarize
myself with it. There were process descriptions, technical documentation and a large
number of best practices, too.
I always had the support of at least one of the experienced security engineers, who turned
out to be very good mentors for me. Despite this, I generally always tried to solve problems
myself and only sought advice on important decisions or questions. The “dailies” (short
daily meetings of the engineers) were perfect for getting questions out of the room and
working effectively on my projects. Every Tuesday we also had a weekly meeting with the
whole team. There, all the sub teams talked about their progress, problems or other topics.
I really liked the icebreaker at the beginning of the meeting where we used this website
most of the time (https://tscheck.in) to generate a random question to answer, as it always
cheered up the atmosphere and allowed us to get to know our team better. Every few weeks,
a team member prepared a knowledge-session for the whole team addressing a specific
security-related topic. Unfortunately, these knowledge-sessions were rather rare due to
lack of capacity, but they were always welcome. In addition, I had a 1:1 meeting with my
manager Georgette every week for the first few months and later every 3 weeks with our
team leader. The atmosphere was always very relaxed, and we talked mainly about my
general satisfaction with the tasks and whether there were any problems or anything else
relevant to my work.
The main tool of communication was Slack. If I had a question, I could simply write it in
one of the channels and I always got quick answers. Spontaneous Zoom calls or short
personal conversations were also possible at any time if there was a need to talk in more
detail.
Before I could push code into the "main branch", a "code review" was mandatory.
However, this is the general standard in the company and a form of quality assurance and
had nothing to do with my position as a working student. The reviewers always provided
constructive and useful feedback, and I was often able to learn something in the process.
All in all, I really liked this way of working. I was treated as a fully-fledged team member,
not as someone who constantly needed guidance. The trust that was placed in me motivated
me and helped me to grow quickly. At the same time, it was always clear to me that I was
not alone and that there was always someone on hand if I had any questions.
8
4. Links between my studies and my internship
I would basically say that up to this point, my studies have given me a solid foundation for
basic work as a developer. For example, I learned how to work with version control
systems like git in courses like "Mobile Operating Systems and Networks" or
"Programming". As we rely entirely on Github at Scout24, this was essential for quick and
easy familiarization. I also learned how to write maintainable, scalable and readable code
in courses such as "Programming" and "Software Engineering". Of course, I was able to
apply this in some implementation projects, although these were often smaller
implementations where scalability, for example, did not play a role. One of the most
important courses was "Cloud Computing", although I only started this in the course of my
work at Scout24. Since we mainly rely on AWS internally and "Cloud Computing" was all
about AWS, this course helped me to understand and work with our cloud infrastructure. I
learned a lot of project management-related things, such as working with Jira or
Confluence, bit by bit on the job. The "Project Management" course dealt more with
theoretical aspects that were only partially useful to me, as practice and theory often differ
greatly, especially in project management, and vary from company to company. We got an
introduction to cyber security in the "Operating Systems and Networks" course. This was
hardly enough to give me a comprehensive grounding in cyber security. Nevertheless, I
found these basics helpful (e.g. encryption or man-in-the-middle attacks). Logically,
however, I learned most about cyber security at Scout24.
I think that working at Scout24 has prepared me very well for possible future careers in the
IT sector. I gained practical experience in a very large company with complex processes
and procedures and was able to look behind the façade of the entire security infrastructure.
Thanks to the combination of studying and working, I know both the theory and the
practice. The work will also help me for my further studies. I suspect it will be particularly
useful in the "Data protection and data security" module, but possibly also in one of the 3
compulsory elective modules that I will take in the 5th semester. The work will also be
very useful for my bachelor’s thesis. However, this is mainly due to the fact that I plan to
write the bachelor thesis directly in the company at Scout24 in order to be able to work as
closely as possible on current problems.
5. Summary and Outlook
I really enjoyed working at Scout24 and was able to learn a lot during this time. I had a
super nice and competent team at my side that always supported and encouraged me. I was
given a lot of confidence from the start and was given more and more responsibility as time
went on.
I wouldn't say that there were any major problems over the entire period. As already
mentioned, the assignment to the authorization groups took some time. Apart from that,
you could also mention that our team was relatively small and sometimes worked above
capacity or pretty much at the limit, which meant that knowledge sessions, for example,
were sometimes a little too short, at least for someone who is still pretty much at the
beginning of the training and wants to learn as much as possible. But such circumstances
9
can always occur and give you a good deal of experience. There were also positive aspects,
namely that I was able to work on many different projects that I might not have been able
to get in a larger team. My suggestions for improvement arise from these problems:
Structured onboarding documentation specifically for new working students would help to
keep track of all necessary steps such as authorizations and tool access centrally. Despite
the high team workload, regular knowledge-sharing sessions would be valuable - these
could be designed as compact "Lunch & Learn" formats to keep the time burden low. The
opportunity to gain insights into neighboring teams such as DevOps or Development
through short rotations would be particularly enriching. In my opinion, this would
strengthen the understanding of overall processes and improve cross-departmental
collaboration. In addition, a small budget for further training, security certifications or
attending specialist conferences, would promote the professional development of working
students in general. These suggestions are not intended to criticize the very positive
working environment, but in my view show some potential for further improving the
already good support for students or newcomers in general.
My time at Scout24 has shown me that I can very well imagine a professional future in the
security industry and that it offers a relatively high level of job security compared to normal
developer jobs, especially in times of AI. I would still consider myself to be relatively new
to the field of cyber security. That's why I thought about pursuing a master’s degree in
cyber security after my bachelor’s degree. Ideally, I would like to do this while working
for such a nice and interesting team and innovative company like Scout24.
10