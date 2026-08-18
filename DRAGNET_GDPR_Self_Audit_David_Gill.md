# DRAGNET GDPR Self-Audit

## Personal Data Inventory

DRAGNET processes personal data submitted by Telegram users and information about identifiable people found during news research. The inventory below distinguishes data that is necessary for the service from metadata that Telegram and n8n receive automatically but DRAGNET does not use.

| Personal data                                    | Data subject                                             | Source                                       | Purpose                                                                                                                          | Storage and retention                                                                                                                                                                                   | Sensitivity and evidence                                                                                                                                                               |
| ------------------------------------------------ | -------------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Telegram user and chat ID                        | Bot user                                                 | Telegram                                     | The chat ID routes the completed report to the correct Telegram conversation.                                                    | Retained in Telegram and saved n8n execution history. It can be manually deleted from n8n, but the Ironhack instance uses default retention settings whose duration has not been verified.              | Ordinary personal data and a persistent identifier. Processing and n8n storage are confirmed by screenshots of a past saved execution.                                                 |
| User-submitted query                             | Bot user and any identifiable person mentioned           | Telegram user                                | Used to search for evidence, assess the claim and generate the report.                                                           | Retained in Telegram, saved n8n execution history and the returned report. A local report copy has no defined retention period and is manually deleted when no longer required.                                                      | May contain ordinary personal data, special-category data, allegations or criminal-offence data. Processing and retention are confirmed.                                               |
| Telegram profile-name fields                     | Bot user                                                 | Telegram profile                             | No DRAGNET purpose has been identified. The fields arrive automatically in the Telegram trigger payload.                         | Retained in Telegram and saved n8n execution history under fields labelled `first_name` and `last_name`.                                                                                                | The values may be a real name, nickname or pseudonym. My profile values are “Scouse Lebowski,” not my legal name. Receipt and retention are confirmed by a saved-execution screenshot. |
| Language code                                    | Bot user                                                 | Telegram                                     | No DRAGNET purpose has been identified. It arrives automatically in the trigger payload.                                         | Retained in saved n8n execution history. The retention duration is unverified.                                                                                                                          | Ordinary personal data when linked to the Telegram ID. Receipt and retention are confirmed by screenshot.                                                                              |
| Message ID, update ID and chat type              | Bot user                                                 | Telegram                                     | No use has been identified in the documented workflow. The chat ID, rather than these identifiers, is used to return the report. | Retained in saved n8n execution history under the default instance settings.                                                                                                                            | Ordinary technical metadata. Receipt and retention are confirmed by screenshot.                                                                                                        |
| Message timestamp                                | Bot user                                                 | Telegram                                     | Supports message delivery and conversation chronology. DRAGNET does not otherwise use it for research.                           | Displayed in Telegram and retained as a Unix timestamp in saved n8n execution history.                                                                                                                  | Ordinary personal data that reveals when the user interacted with the service. Processing and retention are confirmed by screenshots.                                                  |
| Retrieved news and web evidence                  | Identifiable people discussed in retrieved sources       | GNews and OpenAI web search                  | Used to compare sources and assess the submitted claim.                                                                          | Evidence is processed during the workflow. The final report retains DRAGNET’s summary and source links rather than reproducing retrieved article snippets. Vendor-side retention has not been verified. | May include ordinary personal data, special-category data, allegations or criminal-offence data. The final-report format is confirmed; external retention remains an evidence gap.     |
| Generated classification and explanatory summary | Identifiable people discussed in the claim               | Generated by DRAGNET from retrieved evidence | Communicates whether claims are confirmed or strongly supported, disputed, unsupported or unresolved.                            | Retained in the report returned through Telegram. Local reports have no defined retention period and are manually deleted when no longer required.                                                                                  | May create inferences about a person, including sensitive matters. The classifications and storage locations are confirmed.                                                            |
| Returned report and file metadata                | Bot user and identifiable people discussed in the report | DRAGNET, n8n and Telegram                    | Delivers the completed report and records the Telegram file transaction.                                                         | Telegram and saved n8n execution history retain the report filename, file size, file identifiers, return timestamp and associated chat ID. A local report copy has no defined retention period and is manually deleted when no longer required.      | The report may contain personal or sensitive information. The metadata is confirmed by a screenshot of the Send Document node from a saved execution.                                  |
| IP address                                       | Potentially the bot user or connecting service           | Infrastructure layer                         | No DRAGNET purpose has been established. It may be processed for network delivery, security or logging.                          | No IP address appears in the documented Telegram-to-n8n payload. Possible processing in FastAPI or Cloudflare logs has not been evidenced.                                                              | Not currently confirmed as part of DRAGNET’s recorded dataset. Infrastructure logging requires verification.                                                                           |

### Data-minimisation assessment

The chat ID and query text are necessary for DRAGNET to research a claim and return the report. However, Telegram profile-name fields, language code, message ID, update ID and chat type are automatically received and retained in saved n8n executions without an identified DRAGNET purpose.

This creates a data-minimisation concern. I should determine whether n8n can avoid saving unnecessary trigger fields or whether execution data can be deleted automatically after a short, defined period.

### Storage-limitation assessment

Local reports have no defined retention period and remain stored until I manually delete them. Some early test reports have been deleted. Saved n8n executions can also be deleted manually, but I have not configured an automatic deletion period and the default Ironhack retention period has not been verified.

DRAGNET therefore lacks a documented retention schedule. A proportionate schedule should distinguish between:

1. Temporary workflow and execution data.
2. Reports returned to users.
3. Locally retained reports needed for testing or accountability evidence.

### Accuracy and sensitivity

DRAGNET relies on publicly available news and web sources, but public availability does not prevent information from being personal data. Users are not technically restricted from submitting claims about private individuals.

Queries, evidence and reports may contain special-category data under Article 9, such as political opinions, health information, religious beliefs, ethnicity or sexual orientation. They may also contain allegations or information concerning criminal offences, which requires separate consideration under Article 10.

### Evidence

The following evidence currently supports this inventory:

* Screenshot of the Telegram conversation showing query text, dates, timestamps and returned report files.
* Screenshot of a past saved Telegram Trigger execution showing the Telegram ID, profile-name fields, language code, chat type, message and update IDs, timestamp and query text.
* Screenshot of the Send Document node showing use of the chat ID and retention of returned-report metadata.
* The known structure of locally saved DRAGNET reports, which reproduce the query and contain DRAGNET’s summary, classifications and source links.

Remaining evidence gaps include the Ironhack n8n default retention period, vendor retention arrangements, international-transfer safeguards and possible Cloudflare or FastAPI infrastructure logs.


## Controller and Processor Role Map

Under GDPR, roles depend on the actual processing arrangement rather than the technology’s name. A controller determines why and how personal data is processed, while a processor handles it on the controller’s behalf. The following classifications distinguish confirmed roles from those requiring contractual verification.

| Organisation or component                  | Role assessment                                                                     | Reasoning and evidence                                                                                                                                                                                                                                                                    | Outstanding issue                                                                                                                                                                                                                                   |
| ------------------------------------------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| David Gill                                 | Controller                                                                          | I determine DRAGNET’s purpose, evidence-gathering workflow, report generation and local retention. I created and control the Telegram bot, GNews account and Cloudflare tunnel.                                                                                                           | As controller, I am responsible for establishing lawful bases, transparency, retention and suitable safeguards.                                                                                                                                     |
| Ironhack-hosted n8n                        | Ironhack is a probable processor; precise role unverified                           | Ironhack supplies the hosted n8n instance, which receives Telegram payloads, orchestrates DRAGNET and stores execution data. I determine DRAGNET’s purpose and workflow. n8n is the workflow technology, not a separate actor in this Ironhack-hosted deployment.                            | I have not reviewed an Ironhack data-processing agreement, privacy terms, retention policy or subprocessor information. Its role cannot be treated as fully confirmed without this evidence.                                                        |
| Telegram                                   | Separate controller for its platform processing                                     | I created and control the DRAGNET bot through BotFather. Telegram operates the platform and Bot API, manages user accounts and processes messages and chat histories under its own arrangements. I remain controller for DRAGNET’s subsequent use of data obtained through the bot.       | Telegram’s precise responsibilities for bot interactions should be checked against its applicable terms. Telegram states that third-party bot developers are independent from Telegram.                                                             |
| OpenAI                                     | Probable processor or subprocessor                                                  | DRAGNET sends query-related content and evidence to OpenAI using an individual API key assigned to me by Ironhack. OpenAI’s Data Processing Addendum states that OpenAI acts as processor for customer data covered by the agreement.                                                     | Ironhack controls the underlying API account, so it must be established whether OpenAI processes the data on behalf of Ironhack, me, or through a controller-processor-subprocessor chain. The applicable Ironhack agreement has not been reviewed. |
| GNews API, operated by Gigathink Sàrl      | Independent controller for its API processing                                       | I created the free-tier account and accepted its terms. DRAGNET sends search terms to GNews, which may contain personal data. GNews states that it records API queries and request parameters and identifies itself as controller for personal information processed through its service. | Its stated indefinite retention of API usage logs creates a storage-limitation concern where queries contain personal data.                                                                                                                         |
| Cloudflare                                 | Processor for tunnel traffic; separate controller for some account-level processing | I created and controlled the Cloudflare tunnel. It transmitted requests between Ironhack’s hosted n8n workflow and the FastAPI endpoint on my computer. Cloudflare states that it acts as processor for customer and end-user content passing through its services.                       | The applicable account terms, tunnel logging, retention settings and international-transfer safeguards should be verified.                                                                                                                          |
| FastAPI and Uvicorn                        | Software components, not separate GDPR actors                                       | DRAGNET’s FastAPI application ran locally on my computer through Uvicorn. The Cloudflare tunnel exposed that local endpoint so n8n could send the query and receive the report.                                                                                                           | Any local application logs remain processing under my responsibility as controller. Their contents and retention should be checked.                                                                                                                 |
| Telegram users and people named in reports | Data subjects, not service providers                                                | Bot users provide queries and associated Telegram data. Other identifiable people may appear in queries, retrieved evidence and generated reports.                                                                                                                                        | They may exercise applicable GDPR rights, even when the information was obtained from public sources.                                                                                                                                               |

### Confirmed data flow and responsibility

1. A user submits a query to the DRAGNET Telegram bot.
2. The Telegram Trigger node in Ironhack’s hosted n8n instance retrieves the message payload through Telegram’s Bot API.
3. n8n sends the required query to DRAGNET’s FastAPI endpoint through the Cloudflare tunnel.
4. DRAGNET searches GNews and uses OpenAI-powered verification.
5. DRAGNET generates a report and returns it to n8n.
6. n8n’s Telegram Send Document node sends the report through the bot to the correct chat.
7. I retain a copy of the report locally, while Telegram and n8n retain additional data under their respective arrangements.

### Role-map finding

I am the controller for DRAGNET’s core processing because I determine its purpose and essential operation. Ironhack is probably a processor for the hosted n8n workflow, but this requires contractual confirmation. OpenAI’s position depends partly on Ironhack’s API agreement. Telegram and GNews conduct processing under their own service arrangements and should not simply be labelled as DRAGNET processors. Cloudflare acts as a processor for content transmitted through the tunnel, while it may act separately as controller for account, security and service-administration data.

The principal accountability gap is the absence of reviewed contracts or data-processing terms covering Ironhack’s n8n environment and OpenAI account. These documents are necessary to confirm instructions, retention, deletion, security, subprocessors and international-transfer safeguards.

### Supporting sources

* [European Data Protection Board, Guidelines 07/2020 on controller and processor concepts](https://www.edpb.europa.eu/documents/guideline/guidelines-072020-on-the-concepts-of-controller-and-processor-in-the-gdpr_en)
* [Telegram Privacy Policy, bots](https://telegram.org/privacy)
* [OpenAI Data Processing Addendum](https://openai.com/policies/data-processing-addendum/)
* [GNews API Privacy Policy](https://gnews.io/legal/privacy-policy)
* [Cloudflare Privacy Policy](https://www.cloudflare.com/privacypolicy/)

## Lawful Basis Assessment

DRAGNET requires a lawful basis under Article 6 GDPR for each processing purpose. Legitimate interests under Article 6(1)(f) is the strongest provisional basis for necessary ordinary personal data. However, it requires three cumulative tests:

1. A specific and lawful legitimate interest.
2. Processing that is necessary to achieve that interest.
3. A balance showing that the individual’s rights and interests do not override it.

DRAGNET does not request consent, provide terms or create an explicit contractual relationship. Consent and contractual necessity should therefore not be presented as its current lawful bases.

### Legitimate interest pursued

> I have a legitimate interest in developing, testing and evaluating DRAGNET as an academic news-verification prototype. Users also have an interest in receiving evidence-based assessments of claims they voluntarily submit.

DRAGNET currently has no paying customers or commercial use. This reduces its scale but does not remove its GDPR obligations.

### Processing-purpose assessment

| Processing purpose                                                       | Proposed lawful basis                                   | Necessity and balancing assessment                                                                                                                                                                                                          | Outcome                                                                                                                                 |
| ------------------------------------------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Processing the query to investigate the user’s claim                     | Article 6(1)(f), legitimate interests                   | The query is necessary to identify and investigate the claim. A user who voluntarily submits it can reasonably expect this immediate processing. Risks increase where the query concerns another person, particularly a private individual. | Provisionally supportable for ordinary personal data, subject to safeguards and a documented balancing test.                            |
| Processing the chat ID to return the report                              | Article 6(1)(f), legitimate interests                   | The chat ID is necessary to route the report to the correct Telegram conversation.                                                                                                                                                          | Provisionally supportable and proportionate.                                                                                            |
| Generating and returning the report                                      | Article 6(1)(f), legitimate interests                   | This directly fulfils DRAGNET’s news-verification purpose and the user’s request. Inaccurate classifications could nevertheless cause reputational harm.                                                                                    | Provisionally supportable for ordinary personal data if reports contain clear limitations and source evidence.                          |
| Receiving profile-name fields, language code and unused message metadata | No adequate basis presently demonstrated for retention  | Telegram supplies these fields automatically, but DRAGNET does not use them. Retaining them in saved n8n executions is not necessary for claim verification or report delivery.                                                             | Fails the necessity test unless a specific purpose is established. The data should not be retained or should be deleted promptly.       |
| Retaining n8n execution data                                             | Article 6(1)(f) may support limited technical retention | Short-term retention may be necessary for debugging, security and accountability. Default retention of unnecessary payload fields without a defined period is harder to justify.                                                            | A short retention period and automated pruning should be established.                                                                   |
| Retaining local reports for testing and accountability                   | Article 6(1)(f) may support limited retention           | Selected reports may be useful for evaluating DRAGNET, reproducing results and demonstrating development work. Retaining every report without a time limit is not necessary merely because it may become useful later.                      | Potentially supportable with selection criteria, access controls and a defined review or deletion period.                               |
| Including query text in filenames                                        | No necessity demonstrated                               | Current filenames contain the first 60 characters of the sanitised query. This may expose names or allegations without opening the report and is unnecessary for research.                                                                  | Replace the query text with a neutral report identifier, date and timestamp.                                                            |
| Future reuse through Pinecone                                            | No current basis assessed                               | Pinecone is not currently integrated. Reusing historical reports for future retrieval and cost reduction would be a new processing purpose. It could repeatedly surface outdated, inaccurate or sensitive information.                      | Conduct a separate compatibility, lawful-basis and risk assessment before implementation. Do not upload existing reports automatically. |

### Legitimate-interests balancing test

Factors supporting the proposed basis include:

* Users voluntarily submit claims for investigation.
* Processing the query and chat ID directly serves the requested function.
* DRAGNET is currently a limited academic prototype.
* Reports cite sources and distinguish supported, disputed, unsupported and unresolved claims.
* DRAGNET does not make decisions with legal or similarly significant effects.

Factors weighing against it include:

* People discussed in queries did not submit their data and may not expect DRAGNET’s processing.
* Users can submit claims about private individuals.
* Reports are generated without prior human review.
* Inaccurate or unsupported allegations could cause reputational harm.
* Unnecessary Telegram metadata is retained.
* There is no defined retention schedule.
* The bot provides no privacy information.
* Group access has not been technically ruled out.
* Query-derived filenames may reveal personal information.

### Provisional balancing conclusion

Legitimate interests may support the proportionate processing of ordinary personal data needed to investigate a user-submitted news claim and return the report. It does not justify every aspect of the current workflow.

The balance is most favourable where the claim concerns a public matter, the information comes from credible public sources, the report is appropriately qualified and unnecessary data is promptly deleted. It becomes substantially weaker for private individuals, unsupported allegations, sensitive information and indefinite retention.

A written legitimate-interests assessment should be completed after implementing data minimisation, retention limits, privacy information and restrictions on high-risk queries.

### Special-category data under Article 9

Queries, evidence and reports may reveal health information, political opinions, religious beliefs, ethnicity or sexual orientation. Article 6(1)(f) is not sufficient by itself for these categories. An additional Article 9(2) condition is required.

Article 9(2)(e) may apply where the information was manifestly made public by the data subject. It cannot automatically be applied merely because information appears in a news article or was published by somebody else.

Article 9(2)(j), concerning scientific or historical research, should not be claimed solely because DRAGNET is an academic project. It requires an applicable basis in EU or Member State law and appropriate Article 89 safeguards.

A Telegram user also cannot consent on behalf of another person discussed in a query.

DRAGNET currently has no detector, restriction or warning for sensitive queries about private individuals. It therefore cannot demonstrate an Article 9 condition for every query it permits. This is a material lawful-basis gap.

### Criminal-offence data under Article 10

DRAGNET may process allegations, criminal offences or conviction information. Article 10 requires processing under official authority or where EU or Member State law specifically authorises it with appropriate safeguards.

DRAGNET does not currently operate under official authority and has not identified a specific legal authorisation covering unrestricted processing of criminal allegations. Legitimate interests alone does not resolve this requirement.

Queries alleging criminal conduct, particularly by private individuals, should therefore be restricted or subjected to additional legal assessment and safeguards.

### Consent and contractual necessity

Consent is not the current basis because DRAGNET does not request it. Adding a button labelled “I consent” would not solve the processing of information about third parties who have not consented.

Article 6(1)(b), contractual necessity, is also unsuitable at present because the service is free, no terms are accepted and no explicit service agreement exists. The fact that a user requests a report does not justify presenting all downstream retention and reuse as contractually necessary.

### Transparency finding

DRAGNET currently gives users no privacy notice explaining:

* Who controls the processing.
* What data is collected.
* Why it is processed.
* Which services receive it.
* How long it is retained.
* What rights users have.
* Whether information may concern third parties or be transferred internationally.

This transparency failure does not automatically eliminate a possible legitimate interest, but it weakens the balancing assessment because users cannot form reasonable expectations about the full processing chain.

### Required corrective actions

1. Add a concise privacy notice before or when processing begins.
2. Complete and document a formal legitimate-interests assessment.
3. Stop retaining unused Telegram profile and message metadata where technically possible.
4. Configure a defined n8n execution-retention period.
5. Establish criteria and review periods for locally retained reports.
6. Replace query-derived filenames with neutral identifiers.
7. Restrict or review sensitive and criminal-allegation queries, particularly those concerning private individuals.
8. Verify that the bot cannot be used in Telegram groups if group use is not intended.
9. Conduct a separate assessment before introducing Pinecone or reusing historical reports.

### Conclusion

DRAGNET has a plausible Article 6(1)(f) basis for the limited processing of ordinary personal data necessary to investigate a voluntarily submitted news claim and return the resulting report. Its current processing is not fully justified, however, because unnecessary metadata is retained, storage periods are undefined and users receive no privacy information.

The most serious unresolved issues concern special-category data under Article 9 and criminal-offence data under Article 10. Until appropriate conditions and safeguards are established, DRAGNET should not permit unrestricted processing of sensitive allegations about private individuals.

### Supporting sources

* [GDPR, including Articles 6, 9 and 10](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX%3A02016R0679-20160504)
* [European Data Protection Board, Guidelines 1/2024 on legitimate interests](https://www.edpb.europa.eu/system/files/2024-10/edpb_guidelines_202401_legitimateinterest_en.pdf)
* [European Data Protection Board summary of the legitimate-interests test](https://www.edpb.europa.eu/system/files/2024-10/edpb_summary_202401_legitimateinterest_en.pdf)


## Risks, Data-Subject Rights and DPIA Assessment

This assessment covers DRAGNET as it currently operates: a small academic MVP tested by approximately three other people. It does not assess a hypothetical public or commercial version, or the planned Pinecone integration.

Known testing has focused on public figures and news events rather than private individuals. Reports are delivered through private one-to-one Telegram chats, and no privacy incident, complaint, accidental disclosure or known harm has occurred.

### Current safeguards

DRAGNET includes several safeguards within its autonomous workflow:

* It separates confirmed, disputed, unsupported and unresolved claims.
* It cites the evidence used.
* It explains confidence scores and research limitations.
* It identifies evidence that would be needed for a firmer conclusion.
* It uses automated validation and revision stages.
* When validation remains unsuccessful, it delivers a detailed **Automated Validation Warning** explaining that the output is an unresolved draft requiring further research or review.
* Local reports are accessible only through my password-protected Windows account.
* Local reports are not synchronised with external cloud storage.
* Reports are returned privately to the requesting Telegram user.

These controls reduce the risk of false certainty, public disclosure and unauthorised local access.

### Risk assessment

| Risk                                                                      | Current likelihood and impact                                                                                                              | Existing control                                                                                           | Further proportionate action                                                                        |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| An inaccurate conclusion harms a person’s reputation                      | Current likelihood is low because testing is extremely limited, but impact could be significant if a report concerns a serious allegation. | Cautious classifications, source citations, confidence explanations, limitations and automated validation. | Add an AI-generated label and retain prominent uncertainty warnings.                                |
| A user mistakes an autonomous report for human-reviewed fact-checking     | Possible because reports do not currently state that they were generated without human review.                                             | The report explains evidence quality and unresolved claims.                                                | Label every report as autonomously AI-generated without human review.                               |
| A user overlooks a failed-validation warning                              | Possible because the reviewed warning appears after the substantive report.                                                                | Detailed warning explains every outstanding validation problem.                                            | Repeat a concise warning at the beginning of every failed report.                                   |
| Sensitive or criminal allegations about private individuals are processed | No known test has involved a private individual, but the bot does not prevent such queries. Potential impact is high.                      | Reports qualify uncertain claims, but there is no input restriction.                                       | Add a warning or restriction covering sensitive and criminal allegations about private individuals. |
| Unnecessary Telegram metadata is retained                                 | Confirmed in saved n8n executions. Impact is presently limited by the very small number of testers.                                        | Executions can be manually deleted.                                                                        | Establish short retention and remove unnecessary fields where technically possible.                 |
| Query content is exposed through filenames                                | Confirmed because filenames contain the first 60 characters of the sanitised query.                                                        | Files are accessible only through my password-protected Windows account.                                   | Replace query-derived filenames with a neutral report ID.                                           |
| Reports or execution data are retained without consistent criteria        | Confirmed. Some early reports were deleted for organisational reasons, not under a privacy rule.                                           | Manual deletion is possible.                                                                               | Define a simple retention and deletion rule proportionate to MVP testing.                           |
| Data is disclosed through Telegram, Ironhack, OpenAI, GNews or Cloudflare | These services form part of the current processing chain. Their contractual, retention and transfer arrangements are not fully verified.   | Processing is limited in scale and reports are privately delivered.                                        | Verify the most relevant provider terms before broader testing or deployment.                       |
| Local files are accessed without authorisation                            | Present likelihood is low because only I use the password-protected Windows account.                                                       | Windows account access control and no cloud synchronisation.                                               | Continue limiting access; consider additional protection if the project expands.                    |
| Local reports are lost                                                    | Possible because the files are not backed up. Privacy impact is low, although development evidence could be lost.                          | No additional copy exists.                                                                                 | Back up only selected, necessary reports if required, using appropriate access controls.            |

### Autonomous processing and Article 22

DRAGNET operates autonomously and reports are delivered without human review. However, it does not decide whether a person receives employment, credit, healthcare, public benefits or another service, and its classifications do not themselves produce legal or similarly significant effects.

Article 22’s restriction on solely automated decisions with legal or similarly significant effects therefore does not appear to apply to the current MVP. Autonomous operation still creates accuracy and transparency risks that should be controlled.

### Data-subject rights

DRAGNET has no formal process for receiving or handling GDPR requests. If a person requested access, correction, deletion, restriction or objected to processing, I would need to search manually across saved n8n executions, local reports and relevant Telegram data.

The principal applicable rights may include:

* Access to personal data and related processing information.
* Correction of inaccurate personal data.
* Erasure where no lawful reason for continued retention applies.
* Restriction of processing in relevant circumstances.
* Objection to processing based on legitimate interests.
* The right to complain to the relevant supervisory authority.

Data portability is less likely to apply because DRAGNET does not currently rely on consent or contractual necessity, which are conditions relevant to that right.

A reply disputing a result is currently treated as another research query. The bot has no dedicated correction, deletion, objection or complaint function. Given the present scale, a sophisticated automated rights portal would be disproportionate. However, a simple contact method and manual procedure should be documented before testing expands.

### Transparency and correction mechanisms

The reviewed reports explain evidential uncertainty but do not explicitly state that they are autonomously AI-generated without human review.

A proportionate report label would be:

> **AI-generated report:** Produced autonomously by DRAGNET without human review. Check important conclusions against the cited sources.

For failed reports, a concise validation warning should appear at the top as well as the detailed explanation at the end.

Before broader use, the bot should also provide a simple way to request help, correction or deletion. This could initially be handled manually rather than through a complex automated system.

### DPIA threshold

Article 35 requires a DPIA where processing is likely to create a high risk to people’s rights and freedoms. Relevant warning signs include automated evaluation, innovative technology, sensitive information and criminal-offence data. Mandatory examples particularly include large-scale sensitive-data processing and automated evaluation used for decisions producing legal or similarly significant effects. [GDPR, Article 35](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX%3A32016R0679)

DRAGNET uses innovative autonomous processing and could receive sensitive allegations. However, the current circumstances substantially limit its risk:

* It is an academic MVP.
* Approximately three other people have tested it.
* Known tests concern public figures and news events.
* It is not commercially or publicly deployed.
* It does not process sensitive data on a large scale.
* It does not produce decisions with legal or similarly significant effects.
* Reports are returned through private chats.
* No incident or harm has occurred.

### DPIA conclusion

A formal DPIA is **unlikely to be mandatory for DRAGNET in its current limited form**. A lightweight written risk assessment, such as this section, is proportionate and demonstrates that the relevant risks have been considered.

This conclusion should be revisited if the project expands materially, particularly if it:

* Becomes publicly available or attracts substantially more users.
* Regularly processes sensitive or criminal allegations.
* Targets or profiles private or vulnerable individuals.
* Integrates Pinecone and systematically reuses historical reports.
* Publishes or distributes reports beyond the requesting user.
* Influences decisions with significant consequences for individuals.

The EDPB advises assessing factors including scale, sensitivity, automated decision-making, evaluation, vulnerable people and innovative technology when deciding whether a DPIA is required. [EDPB DPIA guidance](https://www.edpb.europa.eu/documents/guideline/data-protection-impact-assessments-high-risk-processing_en)

### Overall finding

DRAGNET’s present privacy risk is limited mainly by its extremely small testing scale, private delivery and careful communication of uncertainty. Its most important current gaps are the absence of an AI-generated label, the placement of validation warnings, unnecessary metadata retention, query-derived filenames and the lack of a simple manual procedure for privacy or correction requests.

These gaps should be documented honestly as improvements for any wider deployment. They do not require treating the current Ironhack MVP as though it were already a mature public service.


## AI Act, ePrivacy and Data Act Cross-Check

This cross-check assesses DRAGNET only as it currently operates: a text-based academic MVP, privately shared with approximately three testers and available only when its local application, Cloudflare tunnel and n8n workflow are connected.

## EU AI Act

### Risk classification

DRAGNET does not perform any prohibited AI practice. It does not:

* Manipulate users through subliminal or deceptive techniques.
* Conduct social scoring.
* Use biometric identification or categorisation.
* Infer emotions.
* Predict criminal behaviour.
* Scrape facial images.
* Exploit children or other vulnerable groups.

It also does not fall within the identified high-risk areas. It is not used for employment, education, credit, essential services, law enforcement, migration, justice, critical infrastructure or safety components of regulated products.

DRAGNET is therefore best classified as an AI system subject primarily to **transparency obligations**, rather than a prohibited or high-risk system.

### Provider and deployer position

I designed DRAGNET’s purpose, workflow, validation logic and user-facing report format, and I operate it under the DRAGNET name. I am therefore provisionally treated as the provider of the assembled DRAGNET system and also its deployer during testing.

OpenAI supplies the underlying model through API access, while Ironhack supplies the API account and hosted n8n environment. Their responsibilities exist alongside, rather than replacing, my responsibility for DRAGNET’s user-facing operation.

### Article 50 interaction disclosure

Article 50(1) requires providers of AI systems that interact directly with people to ensure that users are informed that they are interacting with AI, unless this is obvious from the circumstances. These transparency rules have applied since 2 August 2026. [European Commission Article 50 guidance](https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act)

DRAGNET currently fails this transparency requirement because:

* The bot’s name does not clearly identify it as AI.
* Its description and welcome message do not disclose that it is an AI system.
* Reports do not state that they were generated autonomously without human review.
* The bot may be technically discoverable even though it has only been privately shared.

A proportionate correction would be to display this information when the user first opens or starts the bot:

> **DRAGNET is an autonomous AI news-verification prototype. It researches and generates reports without human review. Its conclusions may be incomplete or incorrect, so check important findings against the cited sources.**

Every report should also contain a shorter label:

> **AI-generated report: produced autonomously by DRAGNET without human review.**

### AI-generated text on matters of public interest

Article 50(4) addresses AI-generated text published for the purpose of informing the public about matters of public interest where there is no human review or editorial control.

DRAGNET generates news-verification text without human review, and its subject matter may concern public affairs. However, its current reports are returned privately to individual users rather than published to the public. Article 50(4)’s publication-specific disclosure therefore appears less directly applicable to the present MVP.

This should be reassessed if reports are later:

* Published on a website.
* Posted automatically on social media.
* Sent to groups or mailing lists.
* Added to a publicly accessible database.
* Otherwise distributed to inform the general public.

Even where Article 50(4) is not directly triggered, voluntarily labelling every report is the clearest and most proportionate approach.

### Machine-readable marking

Article 50 also includes provider obligations concerning machine-readable marking of AI-generated content. DRAGNET predates 2 August 2026, and the Commission describes a limited transition until 2 December 2026 for Article 50(2) marking obligations applying to systems placed on the market before that date.

The exact technical application to this privately tested, API-based MVP requires further assessment. For the present Ironhack project, a clear visible text label is the immediate priority. Machine-readable marking should be reviewed before any wider deployment.

### Deepfakes and biometric transparency

DRAGNET produces only text. It does not generate or manipulate images, audio or video and does not use emotion recognition or biometric categorisation.

The corresponding deepfake, emotion-recognition and biometric-disclosure provisions are therefore not relevant to the present system.

### AI literacy

Because I design and operate an autonomous AI workflow, I should maintain sufficient understanding of its capabilities, limitations, validation process and foreseeable risks. The documented research logic, confidence qualifications, automated validation and this compliance audit support that requirement.

Any wider deployment would require clearer operational instructions covering:

* Appropriate use of reports.
* Interpretation of confidence and validation status.
* Handling of sensitive or criminal allegations.
* Correction and deletion requests.
* Escalation of harmful or unreliable outputs.

### AI Act conclusion

DRAGNET is not prohibited or high-risk. Its main AI Act issue is transparency.

The necessary MVP improvement is simple but important: clearly inform users at first interaction and within every report that DRAGNET is an autonomous AI system and that its output has not been reviewed by a human. Failed-validation warnings should also appear at the beginning of the report, not only at the end.

The Commission confirms that Article 50 applies from 2 August 2026 and covers interactive AI systems and certain AI-generated content. [European Commission transparency guidelines](https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations)

## ePrivacy cross-check

DRAGNET communicates with users through Telegram, but it does not:

* Send unsolicited messages.
* Send direct marketing or promotional communications.
* Contact users after completing their requested report.
* Use cookies.
* Deploy advertising trackers.
* Use analytics tools such as Google Analytics.
* Access information stored on a user’s device beyond what Telegram supplies through the bot interaction.

DRAGNET only responds when a user voluntarily submits a query. Direct-marketing and cookie-consent rules therefore do not create a significant issue for the current MVP.

Telegram and the infrastructure providers may conduct separate communications and device-level processing under their own arrangements. Those activities should not automatically be attributed to DRAGNET, although the user-facing privacy information should identify Telegram as part of the service chain.

### ePrivacy conclusion

No material direct-marketing or cookie-compliance gap has been identified. The more relevant issue is confidentiality and minimisation of the Telegram data retained in saved n8n executions, which is already addressed under the GDPR assessment.

## EU Data Act cross-check

The EU Data Act focuses substantially on:

* Data generated through connected products and related services.
* User access to product and service data.
* Business-to-business and business-to-government data sharing.
* Unfair contractual terms governing data access.
* Switching and interoperability between data-processing services.

DRAGNET is not a connected product, Internet of Things device or related service. It does not collect product-generated data, act as a data holder for connected-device users or offer a commercial cloud service.

The Data Act therefore imposes no significant direct product-data obligations on the present MVP. [EU Data Act](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=OJ%3AL_202302854)

Cloud and API providers used by DRAGNET may have their own Data Act obligations, particularly concerning switching between data-processing services. Those provider-level obligations do not make DRAGNET itself a cloud-service provider.

A future Pinecone integration would not automatically bring DRAGNET within the connected-product rules. It would instead raise GDPR questions about reuse, retention, accuracy, deletion and portability between service providers.

### Data Act conclusion

The Data Act has little direct relevance to DRAGNET as it currently stands. It should not be presented as a major compliance burden merely because DRAGNET sends data through APIs and cloud infrastructure.

## Overall cross-check conclusion

| Legal framework | Current relevance        | Principal finding                                                                                                                                             |
| --------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EU AI Act       | Directly relevant        | DRAGNET is not prohibited or high-risk, but users are not clearly informed that they are interacting with autonomous AI.                                      |
| ePrivacy rules  | Limited relevance        | No cookies, tracking or unsolicited marketing are used. Telegram confidentiality and metadata are better addressed through GDPR controls.                     |
| EU Data Act     | Minimal direct relevance | DRAGNET is not a connected product, data holder or data-processing-service provider. Provider switching and future Pinecone use are secondary considerations. |

The priority is to fix the AI-transparency gap. The ePrivacy and Data Act checks do not reveal comparable concerns for the current limited MVP.

## Compliance Memo

**Project:** DRAGNET Autonomous News Verification Agent
**Project owner:** David Gill
**Assessment date:** 18 August 2026
**Current status:** Academic MVP under limited testing
**Recommendation:** Continue limited MVP testing with conditions

### Executive summary

DRAGNET is an autonomous AI news-verification prototype that receives a user’s query through Telegram, researches the claim and returns a text report without human review.

The current system has been tested by approximately three other people. Known tests have concerned public figures and news events rather than private individuals. Reports are delivered through private Telegram chats, and no privacy incident, complaint or known harm has occurred.

DRAGNET is not a prohibited or high-risk AI system. Its current scale and use do not justify stopping the project or treating it as a mature commercial service. However, it is not ready for wider public deployment.

The principal compliance gaps concern:

* No clear disclosure that users are interacting with autonomous AI.
* No user-facing privacy information.
* Retention of unnecessary Telegram metadata.
* No defined retention schedule.
* Query text appearing in report filenames.
* No restriction on sensitive or criminal allegations about private individuals.
* No simple correction, objection or deletion procedure.
* Unverified contractual and retention arrangements for Ironhack and some external providers.

### Processing and responsibility

I determine DRAGNET’s purpose, workflow, validation logic, report format and local retention. I am therefore the controller for its core processing.

The current data flow involves Telegram, Ironhack-hosted n8n, a locally operated FastAPI application, Cloudflare, GNews and OpenAI. Ironhack is probably a processor for the hosted workflow, but its precise responsibilities require contractual verification.

Telegram and GNews conduct some processing under their own service arrangements. Cloudflare appears to act as a processor for tunnel traffic while conducting separate account-level processing. OpenAI’s precise position depends partly on Ironhack’s underlying API arrangement.

### Lawfulness

Legitimate interests under Article 6(1)(f) is the strongest provisional lawful basis for the ordinary personal data necessary to:

* Process a voluntarily submitted query.
* Research the requested claim.
* Use the chat ID to return the report.
* Retain limited technical information for debugging and accountability.

That basis does not justify retaining unused Telegram profile fields and message metadata without a defined purpose or retention period.

DRAGNET cannot currently demonstrate an Article 9 condition for every possible query involving special-category data. It has also not established the legal authority required for unrestricted processing of criminal-offence data under Article 10.

Known testing has not involved private individuals, which substantially limits the current demonstrated risk. Nevertheless, sensitive or criminal allegations about private individuals should be restricted before access expands.

### Transparency

DRAGNET provides no privacy notice, terms or explanation of its data processing when a user starts the bot.

Reports disclose sources, disputed claims, confidence and research limitations. Reports that fail automated validation also explain that they remain unresolved drafts and list the outstanding problems. These are meaningful safeguards.

However, neither the bot nor its reports clearly state that DRAGNET is an autonomous AI system operating without human review. This is the clearest immediate AI Act transparency gap.

### Risk and DPIA conclusion

DRAGNET uses innovative autonomous processing and could receive sensitive information. However:

* It is a small academic MVP.
* Only approximately three other people have tested it.
* Known tests concern public figures and news events.
* It has no commercial customers or public launch.
* It does not process sensitive information on a large scale.
* It does not make decisions producing legal or similarly significant effects.
* Reports are delivered privately.
* No privacy incident or known harm has occurred.

A formal DPIA is therefore unlikely to be mandatory for the current MVP. The documented risk assessment is proportionate at this stage. The DPIA position should be reassessed if the project becomes publicly available, processes personal allegations at greater scale or introduces systematic reuse through Pinecone.

### AI Act, ePrivacy and Data Act

DRAGNET is not prohibited or high-risk under the EU AI Act. Its main obligation is transparency because users interact directly with an AI system.

The bot does not send marketing, use cookies, deploy trackers or contact users without a query. No material direct-marketing or cookie issue has therefore been identified under ePrivacy rules.

The EU Data Act has little direct relevance because DRAGNET is not a connected product, related service, data holder or cloud-service provider.

### Required actions during continued MVP testing

1. Add a clear AI disclosure to the bot’s welcome message.
2. Label every report as autonomously AI-generated without human review.
3. Place failed-validation warnings at the beginning as well as the end of reports.
4. Add a concise privacy notice identifying the controller, purposes, recipients, retention and user rights.
5. Replace query-derived filenames with neutral identifiers.
6. Delete unnecessary n8n execution data after testing where practical.
7. Avoid deliberately testing sensitive or criminal allegations about private individuals.

### Actions before wider deployment

1. Define retention periods for n8n executions and local reports.
2. Minimise unused Telegram profile and message metadata.
3. Document a legitimate-interests assessment.
4. Add a simple method for correction, objection and deletion requests.
5. Restrict or review sensitive or criminal allegations about private individuals.
6. Confirm that Telegram group use is disabled.
7. Verify Ironhack’s processing terms, retention arrangements and subprocessor chain.
8. Review international-transfer safeguards for the external services.
9. Reassess whether a formal DPIA is required.
10. Conduct a separate assessment before introducing Pinecone or reusing historical reports.

### Final recommendation

DRAGNET should **continue limited MVP testing with conditions**.

The current project demonstrates valuable safeguards through cautious classifications, source attribution, confidence explanations, research transparency and automated validation. Its very limited testing scale also reduces the present likelihood of harm.

It should not yet be described as fully compliant or ready for unrestricted public use. The immediate priority is to make its autonomous AI nature clear, provide basic privacy information and avoid unnecessary retention. More formal governance and technical controls should be added only as access, data volume and project ambitions expand.

