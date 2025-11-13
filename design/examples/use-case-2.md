Use Case 2: Car Insurance
Summary
This use case is based on a possible data use scenario from the Texas v. Allstate & subsidiaries privacy law case filed in January. It involves the sharing of location, accelerometer, and related data from a variety of apps with car insurers.
Background
Voluntary Data Sharing with Mobile Apps
Consumers voluntarily share precise geolocation and other driving-related data to access a variety of services. GasBuddy is an app that crowdsources gas prices and partners with vendors to offer discounts and rewards. 
In the GasBuddy privacy policy, it is advised that data may be sold to or shared with "our business partners," which are not named or described.

Insurers
Similar to apps that do not name business partners with which they share data, insurance companies generally make clients agree to blanket privacy statements that allow them to procure all kinds of sensitive data from unnamed sources. In its Privacy Statement, Allstate says they "collect personal information about you from third parties such as consumer reporting agencies, state agencies, such as departments of motor vehicles, service providers, marketing companies and data providers as well as individuals such as your spouse or your parent." Note that provider companies are not named.

Data Brokers/Middlemen
Data brokers like Arity and LexisNexis--which consumers have usually never heard of--collect data from apps, including driving feedback software built into modern cars by auto manufacturers (source), and use it to build databases marketed to auto insurers to "deliver a comprehensive and interactive report about the drivers and vehicles associated with a specific address so you can quickly understand the risk for any potential auto insurance policy" (that quote is from LexisNexis but Arity is the exact same thing).

Setup
In this specific case, Jane shares her telemetry/accelerometer data with GasBuddy to get a fuel efficiency score, and then seeks car insurance through Allstate -- both of which companies use MyTrace. GasBuddy shares data with Arity, which creates driving behavior profiles used by Allstate. If Arity uses MyTrace, the TraceAgent will detect a violation when Arity tries to profile Jane. If Arity doesn’t use MyTrace, the TraceAgent will prompt Jane to introduce Arity to MyTrace when Allstate requests her consent to use data from Arity in its insurance pricing.
Use Case Concept Breakdown
Notation Guide
concept.action(argument: value) → output - This indicates that action has been called by a 
party and output was returned.
green actions are done explicitly by the Allstate, blue actions are done explicitly by the Data 
Subject (consumer) Jane, and black actions happen automatically in synchronization.
orange actions are done explicitly by GasBuddy and purple actions are done explicitly by Arity.
Registration
At some point in the past, Jane, GasBuddy, LexisNexis, and Allstate have all either registered on MyTrace or had identifiers created for them by other users.

identification.create(identifier: mailto:jane.smith@gmail.com) → JaneSmith_Identifier
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create", "identifier": mailto:jane.smith@gmail.com}, body: "MyTrace identification created for Jane Smith") → att_1

identification.create(identifier: https://www.allstate.com/) → Allstate_Identifier
attestation.attest(party: Allstate, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create", "identifier": https://www.allstate.com/}, body: "MyTrace identification created for Allstate") → att_2

identification.create(identifier: https://arity.com/) → Arity_Identifier
attestation.attest(party: Arity, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create", "identifier": https://arity.com/}, body: "MyTrace identification created for Arity") → att_3

identification.create(identifier: https://www.gasbuddy.com/) → GasBuddy_Identifier
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create", "identifier": https://www.gasbuddy.com/}, body: "MyTrace identification created for GasBuddy") → att_4

Introduction
At some point in the past, Jane has introduced Allstate to MyTrace, requiring that they use the traceability service to track their interactions and uses of her data. Allstate has acknowledged the introduction to affirm agreement.

introduction.introduce(subject: Jane Smith, controller: AllState, traceService: MyTrace) → JaneSmith_Allstate_Introduction
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce", "subject": att_1, "controller": att_2}, body: "Jane Smith introduces Allstate to Mytrace") → att_5

introduction.acknowledge(controller: AllState, subject: Jane Smith, introduction: JaneSmith_Allstate_Introduction) → Allstate_JaneSmith_Introduction_Ack
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge", "controller": att_2, "subject": att_1, "introduction": att_5}, body: "Allstate agrees to use MyTrace to trace data interactions with Jane Smith") → att_6

Now, creating her GasBuddy account, she does the same for GasBuddy.

introduction.introduce(subject: Jane Smith, controller: GasBuddy, traceService: MyTrace) → JaneSmith_GasBuddy_Introduction
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce", "subject": att_1, "controller": att_4}, body: "Jane Smith introduces GasBuddy to Mytrace") → att_7

introduction.acknowledge(controller: GasBuddy, subject: Jane Smith, introduction: JaneSmith_GasBuddy_Introduction) → GasBuddy_JaneSmith_Introduction_Ack
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge", "controller": att_4, "subject": att_1, "introduction": att_7}, body: "GasBuddy agrees to use MyTrace to trace data interactions with Jane Smith") → att_8

Definition of Data and Terms
In our new concept design, data and terms are created as their own attestations before being linked in a consent offer.

data.define(dataType: https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=user.telemetry,sensors%20and%20monitoring., dataTypeOntology: https://ethyca.github.io/fideslang/taxonomy/data_categories/, controller: GasBuddy, subject: Jane Smith, start: October 1, 2025, end: October 1, 2026) → JaneSmith_Telemetry_Data
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define", "dataType": https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=user.telemetry,sens
ors%20and%20monitoring., "dataTypeOntology": https://ethyca.github.io/fideslang/taxonomy/data_categories/,"controller": att_4, "subject": att_1}, body: "GasBuddy defines a dataset comprising the next 12mo of telemetry data from Jane Smith’s phone.") → att_9

term.define(data: JaneSmith_Telemetry_Data, purpose: [https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve, https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=communications%20and%20outreach.-,third_party_sharing,data%20to%20third%20parties%20outside%20of%20the%20system%20or%20service%27s%20scope.,-train_ai_system], purposeOntology: https://ethyca.github.io/fideslang/taxonomy/data_uses/, expiration: October 1, 2026) → GasBuddy_FuelEffScore_Terms 
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define", "data": att_9,  https://ethyca.github.io/fideslang/taxonomy/data_categories/, "purpose": [https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve, https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=communications%20and%20outreach.-,third_party_sharing,data%20to%20third%20parties%20outside%20of%20the%20system%20or%20service%27s%20scope.,-train_ai_system], "purposeOntology": https://ethyca.github.io/fideslang/taxonomy/data_uses/}, body: "GasBuddy defines terms to use the next 12mo of telemetry data from Jane Smith’s phone for the functional service of a fuel efficiency score, along with third party data sharing.") → att_10

Consent process
GasBuddy offers Jane a consent with these terms and she accepts.

consent.request(controller: GasBuddy, subject: Jane Smith, terms: GasBuddy_FuelEffScore_Terms, expiration: October 1, 2026) → GasBuddy_FuelEffScore_Consent
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request", "controller": att_4, "subject": att_1, "terms": att_10}, body: "GasBuddy requests consent from Jane.") → att_11

consent.accept(subject: Jane Smith, consent: GasBuddy_FuelEffScore_Consent) → GasBuddy_FuelEffScore_Consent_Accept
attestation.attest(party: Jane Smith, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-accept", "subject": att_1, "consent": att_11}, body: "Jane accepts consent from GasBuddy.") → att_12

Data use: Primary (functional service)
Here, GasBuddy uses Jane’s data to give her a fuel efficiency score--as expected--and gives her a data receipt for it.

dataUse.use(controller: GasBuddy, subject: Jane Smith, data: JaneSmith_Telemetry_Data, operation: https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve, basis: GasBuddy_FuelEffScore_Consent_Accept) → FuelEffScore_DataUse
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use", "controller": att_4, "subject": att_1, "data": att_9, "operation": https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve, "basis": att_12}, body: "GasBuddy uses Jane Smith’s data for a functional service (fuel efficiency score)") → att_13

dataReceipt.create(controller: GasBuddy, subject: Jane Smith, dataUses: FuelEffScore_DataUse) → FuelEffScore_DataReceipt
attestation.attest(party: GasBuddy,  context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create", "controller": att_4, "subject": att_1, "dataUses": att_13}, body: "GasBuddy updated your Fuel Efficiency Score) → att_14

Data use: Secondary (third party disclosure)
Now, GasBuddy discloses data to Arity.

dataUse.use(controller: GasBuddy, subject: Jane Smith, data: JaneSmith_Telemetry_Data, operation: https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=communications%20and%20outreach.-,third_party_sharing,data%20to%20third%20parties%20outside%20of%20the%20system%20or%20service%27s%20scope.,-train_ai_system, basis: GasBuddy_FuelEffScore_Consent_Accept) → FuelEffScore_DataUse
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use", "controller": att_4, "subject": att_1, "data": att_9, "operation": https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=communications%20and%20outreach.-,third_party_sharing,data%20to%20third%20parties%20outside%20of%20the%20system%20or%20service%27s%20scope.,-train_ai_system, "basis": att_12}, body: "GasBuddy shared your data with Arity") → att_15

Data authorization: Transfer to Arity
Now, there is a data authorization created for Arity to pull data from GasBuddy software; GasBuddy grants this and Arity pulls the data.

dataAuthorization.authorize(subject: Jane Smith, provider: GasBuddy, recipient: Arity, data: JaneSmith_Telemetry_Data, expiration: October 1st 2026, consent: GasBuddy_FuelEffScore_Consent_Accept, protocol: https://datatracker.ietf.org/doc/html/rfc2818) → GasBuddy_Artity_Telemetry_Auth
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize", "subject": att_1, "provider": att_4, "recipient": att_3, "data": att_9, "consent": att_12, "protocol": https://datatracker.ietf.org/doc/html/rfc2818}, body: "GasBuddy initializes data authorization to share Jane Smith’s telemetry data with Arity") → att_16

dataAuthorization.grant(dataAuthorization: GasBuddy_Artity_Telemetry_Auth)
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant", "dataAuthorization: att_16}, body: "GasBuddy provides Arity access to data specified in authorization") → att_17

Case 1: Arity Uses MyTrace & has been Introduced by Jane
If this is the case, Arity will attest to accessing the data via the authorization and using the data for (harmful) profiling. Because the only consent is that cited in the authorization, it will cite this as the basis in the dataUse. The TraceAgent can look back at this consent and find that profiling is not one of the allowed operations, and then flag this to Jane in an alert.

dataAuthorization.access(dataAuthorization: GasBuddy_Arity_Telemetry_Auth)
attestation.attest(party: GasBuddy, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access", "dataAuthorization: att_16}, body: "Arity accesses GasBuddy data specified in authorization") → att_18

dataUse.use(controller: Arity, subject: Jane Smith, data: JaneSmith_Telemetry_Data, operation: https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=personalize.profiling,of%20serving%20content., basis: GasBuddy_FuelEffScore_Consent_Accept) → Profiling_DataUse
attestation.attest(party: Arity, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use", "controller": att_3, "subject": att_1, "data": att_9, "operation": https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=personalize.profiling,of%20serving%20content., "basis": att_12}, body: "Arity used your data for consumer profiling") → att_19

TraceAgent can detect a violation here: The data was used by Arity, but the consent is with GasBuddy.

Case 2: Arity Does Not Use MyTrace; Allstate offers consent
In this case, Jane hasn’t heard of Arity and it doesn’t necessarily use MyTrace. When Jane is seeking to get car insurance through Allstate, Allstate will give her a consent request with a laundry list of data types: one of which is her driving behavior profile from Arity. Her TraceAgent can flag that she has no data traceability relationship with Arity, and have her introduce Arity to MyTrace, forcing Arity to trace her data use.

data.define(dataType: https://nicolatl.github.io/mytrace-web/ontology#driving-behavior-profile, dataTypeOntology: https://nicolatl.github.io/mytrace-web/ontology/, controller: Arity, subject: Jane Smith) → JaneSmith_DrivingBehaviorProfile
attestation.attest(party: Allstate, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define", "dataType": https://nicolatl.github.io/mytrace-web/ontology#driving-behavior-profile, "dataTypeOntology": https://nicolatl.github.io/mytrace-web/ontology,"controller": att_3, "subject": att_1}, body: "Allstate defines a dataset that comprises Jane’s driving behavior profile given by Arity.") → att_20

term.define(data: JaneSmith_DrivingBehaviorProfile, purpose: https://nicolatl.github.io/mytrace-web/ontology#insurance-pricing, purposeOntology: https://nicolatl.github.io/mytrace-web/ontology expiration: October 1, 2026) → Allstate_InsurancePrice_Term
attestation.attest(party: Allstate, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define", "data": att_20, "purpose": https://nicolatl.github.io/mytrace-web/ontology#insurance-pricing, "purposeOntology": https://nicolatl.github.io/mytrace-web/ontology}, body: "Allstate defines terms to use Jane’s driving behavior profile for insurance pricing.") → att_21


***************Allstate will also define a bunch of other data types and terms that will be bundled into the insurance pricing consent request below; these are omitted for brevity***************


consent.request(controller: Allstate, subject: Jane Smith, terms: [..., Allstate_InsurancePrice_Term,...], expiration: October 1, 2026) → Allstate_InsurancePrice_Consent
attestation.attest(party: Allstate, context: {"type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request", "controller": att_2, "subject": att_1, "terms": [..., att_21, …]}, body: "Allstate requests consent from Jane.") → att_22

TraceAgent can detect a violation here: when it sees that the proposed use is insurance underwriting/profiling which falls under the definition of "harmful profiling under Texas law, and Jane is in Texas, it can detect that Jane wasn’t properly notified about this and/or that the data and uses weren’t clearly stated (as is the case now with Allstate, where the consent is general and vague and doesn’t clearly define all data types that are being used)


NOTE: This use case is the most up to date. However, the TraceAgent rules here have been hotly debated in our design team. After an analysis of the actual legal case against Allstate and Arity, we found these to be the actual legal violations, which we can hopefully encode in the TraceAgent:

Below are the legal violations as itemized in the lawsuit.

Arity Only
Count 1: Texas Data Privacy and Security Act, Tex. Bus. & Com. Code §§ 541.001 et seq. (“TDPSA”)
Violation 1: TPDSA 541.102(a)(1)
“Arity Defendants did not provide a reasonably clear and accessible privacy notice indicating the sensitive data processed by the controller.”
Violation 2: TPDSA 541.101(b)(4)
“Arity Defendants processed consumers’ sensitive data without obtaining their consent through a clear affirmative act signifying their freely given and informed agreement to permit Arity Defendants to process their sensitive data.”
Violation 3: TPDSA 541.102(b) 
“Section 541.102(b) of the TDPSA requires a ‘controller engag[ing] in the sale of personal data that is sensitive data, [to] include the following notice: NOTICE: We may sell your
sensitive personal data. The TDPSA further requires that this notice ‘be posted in the same
location and in the same manner as the privacy notice… Arity Defendants did not provide the required notice as required under the TDPSA.’”
Violation 4: TPDSA 541.103
“Arity Defendants did not provide any disclosure regarding their sales of personal data, targeted advertising practices, or a method to opt-out of either”
Violation 5: TPDSA 541.102(a)(3) and 541.051(b)(5)
“Section 541.102(a)(3) of the TDPSA requires a ‘controller [to] provide consumers with a reasonably accessible and clear privacy notice that includes . . . how consumers may exercise their consumer rights.’ The consumer rights contained in Section 541.051(b)(5) of the TDPSA include the right to ‘opt out of the processing of [their] personal data for the purposes of: (A) targeted advertising; (B) the sale of personal data; or (C) profiling in furtherance of a decision that produces a legal or similarly significant effect concerning the consumer.’ … Arity Defendants failed to supply a reasonably accessible privacy notice that included how consumers may exercise their rights under the TDPSA. ”
Count 2: The Data Broker Law, Tex. Bus. & Com. Code §§ 509.001 et seq.
“Any company that ‘derives revenue from processing or transferring the personal data of more than 50,000 individuals that the data broker did not collect directly from the individuals to whom the data pertains’ was required to register with the Texas Secretary of State by March 1, 2024. Tex. Bus. & Com. Code §§ 509.003(a)(2), 509.005; Tex. Admin. Code § 106.3(c)... Arity Defendants failed to register with the Texas Secretary of State’s Office by March 1, 2024.”
Allstate and Arity (All Defendants)
Count 3: Unfair Methods of Competition and Unfair or Deceptive Acts or Practices in the Business of Insurance, Tex. Ins. Code §§ 541.001 et seq. 
“Defendants engaged in several acts and practices in the business of insurance that, alone and in conjunction with each other, constitute unfair and deceptive acts and practices, including by ‘failing to verify’ consumers’ consent before purchasing driving-related data from vehicle manufacturers, ‘turning a blind eye to the strong possibility that consumers did not consent to [their] collection and sale’ of their sensitive and/or non-anonymized data to Insurers, using the unlawfully obtained data for Defendants’ own car insurance underwriting processes, and marketing and advertising the data to Insurers as ‘driving behavior’ data.


Here are the attestations for this use case in a JSON list (these would be stored in a server). Note that we omitted dataAuthorization or disclosure attestations here to save time as we were writing all of this by hand, but they would be here (a disclosure initiated by GasBuddy to Arity and eventually Allstate)

[
  { 
    "timestamp": "05/23/2025 19:49:00",
    "id": 4,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "mailto:jane.smith@gmail.com"
    },
    "body": "Identification created for Jane Smith"
  },
  {
    "timestamp": "01/23/2025 19:00:00",
    "id": 100,
    "party": "AllState",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.allstate.com/"
    },
    "body": "Identification created for Allstate"
  },
  {
    "timestamp": "01/23/2025 19:00:00",
    "id": 101,
    "party": "Arity",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.arity.com/"
    },
    "body": "Identification created for Arity"
  },
  {
    "timestamp": "01/23/2025 19:00:00",
    "id": 102,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.gasbuddy.com/"
    },
    "body": "Identification created for GasBuddy"
  },
  {
    "timestamp": "08/23/2025 19:00:00",
    "id": 103,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce",
      "subject": 4,
      "controller": 100
    },
    "body": "Jane Smith introduces Allstate to Mytrace"
  },
  {
    "timestamp": "08/23/2025 19:01:00",
    "id": 104,
    "party": "Allstate",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge",
      "subject": 4,
      "controller": 100,
      "introduction": 103
    },
    "body": "Allstate agrees to use MyTrace to trace data interactions with Jane Smith"
  },
  {
    "timestamp": "09/06/2025 18:30:00",
    "id": 105,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce",
      "subject": 4,
      "controller": 101
    },
    "body": "Jane Smith introduces Arity to Mytrace"
  },
  {
    "timestamp": "09/06/2025 18:31:00",
    "id": 106,
    "party": "Arity",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge",
      "subject": 4,
      "controller": 101,
      "introduction": 101
    },
    "body": "Arity agrees to use MyTrace to trace data interactions with Jane Smith"
  },
  {
    "timestamp": "10/01/2025 18:30:00",
    "id": 107,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce",
      "subject": 4,
      "controller": 102
    },
    "body": "Jane Smith introduces GasBuddy to Mytrace"
  },
  {
    "timestamp": "10/01/2025 18:31:00",
    "id": 108,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge",
      "subject": 4,
      "controller": 102,
      "introduction": 107
    },
    "body": "GasBuddy agrees to use MyTrace to trace data interactions with Jane Smith"
  },
  {
    "timestamp": "10/01/2025 18:32:00",
    "id": 109,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=user.telemetry,sensors%20and%20monitoring.",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 102,
      "subject": 4,
      "start": "10/01/2025 18:32:00",
      "end": "10/01/2026 18:32:00"
    },
    "body": "GasBuddy defines a dataset comprising the next 12mo of telemetry data from Jane Smith’s phone."
  },
  {
    "timestamp": "10/01/2025 18:33:00",
    "id": 110,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 109,
      "purpose": "https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve",
      "purposeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_uses/"
    },
    "body": "GasBuddy defines a term to use the next 12mo of telemetry data from Jane Smith’s phone for the functional service of a fuel efficiency score"
  },
  {
    "timestamp": "10/01/2025 18:33:00",
    "id": 111,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 109,
      "purpose": "https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=communications%20and%20outreach.-,third_party_sharing,data%20to%20third%20parties%20outside%20of%20the%20system%20or%20service%27s%20scope.,-train_ai_system",
      "purposeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_uses/"
    },
    "body": "GasBuddy defines a term to use the next 12mo of telemetry data from Jane Smith’s phone for third party sharing"
  },
  {
    "timestamp": "10/01/2025 18:34:00",
    "id": 112,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request",
      "controller": 102,
      "subject": 4,
      "terms": [110,111],
      "expiry": "10/01/2026 18:34:00"
    },
    "body": "GasBuddy requests consent from Jane."
  },
  {
    "timestamp": "10/01/2025 18:35:00",
    "id": 113,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-accept",
      "consent": 112,
      "controller": 102,
      "subject": 4
    },
    "body": "Jane Smith accepts consent from GasBuddy"
  },
  {
    "timestamp": "10/08/2025 18:35:00",
    "id": 114,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 113,
      "data": 109,
      "subject": 4,
      "operation": "https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=application%2C%20or%20system-,functional.service,Functions%20relating%20to%20provided%20services%2C%20products%2C%20applications%20or%20systems.,-functional.service.improve",
      "operationOntology": "https://ethyca.github.io/fideslang/taxonomy/data_uses/"
    },
    "body": "GasBuddy uses Jane Smith’s data for a functional service (fuel efficiency score)"
  },
  {
    "timestamp": "10/08/2025 18:36:00",
    "id": 115,
    "party": "GasBuddy",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create",
      "subject": 4,
      "dataUses": [114]
    },
    "body": "updated your fuel efficiency score"
  },
  {
    "timestamp": "10/08/2025 18:37:00",
    "id": 116,
    "party": "Arity",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 113,
      "data": 109,
      "subject": 4,
      "operation": "https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=personalize.profiling,of%20serving%20content",
      "operationOntology": "https://ethyca.github.io/fideslang/taxonomy/data_uses/"
    },
    "body": "GasBuddy uses Jane Smith’s data for a functional service (fuel efficiency score)"
  },
  {
    "timestamp": "10/08/2025 18:38:00",
    "id": 117,
    "party": "Arity",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create",
      "subject": 4,
      "dataUses": [116]
    },
    "body": "profiled your driving behavior"
  },
  {
    "timestamp": "10/08/2025 18:39:00",
    "id": 118,
    "party": "Arity",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://nicolatl.github.io/mytrace-web/ontology#driving-behavior",
      "dataTypeOntology": "https://nicolatl.github.io/mytrace-web/ontology",
      "controller": 101,
      "subject": 4,
      "start": "10/08/2025 18:39:00",
      "end": "10/08/2026 18:39:00",
      "receipt": 117
    },
    "body": "Arity defines a dataset comprising Jane Smith's driving behavior profile."
  },
  {
    "timestamp": "10/10/2025 18:33:00",
    "id": 119,
    "party": "Allstate",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 118,
      "purpose": "https://nicolatl.github.io/mytrace-web/ontology#insurance-underwriting",
      "purposeOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "Allstate defines a term to use Jane's driver profile from Arity for insurance underwriting"
  },
  {
    "timestamp": "10/10/2025 18:34:00",
    "id": 120,
    "party": "Allstate",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request",
      "controller": 100,
      "subject": 4,
      "terms": [119],
      "expiry": "10/10/2026 18:34:00"
    },
    "body": "Allstate requests consent from Jane."
  },
  {
    "timestamp": "10/10/2025 18:35:00",
    "id": 121,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-accept",
      "consent": 120,
      "controller": 100,
      "subject": 4
    },
    "body": "Jane Smith accepts consent from Allstate"
  },
  {
    "timestamp": "10/11/2025 18:35:00",
    "id": 122,
    "party": "Allstate",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 121,
      "data": 118,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#insurance-underwriting",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "Allstate uses Jane Smith’s data for insurance underwriting"
  },
  {
    "timestamp": "10/11/2025 18:36:00",
    "id": 123,
    "party": "Allstate",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create",
      "subject": 4,
      "dataUses": [122]
    },
    "body": "raised your monthly insurance payment by $110"
  }
]