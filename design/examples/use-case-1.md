Use Case: Lending Traceability
With Attestation Identifier Linking & Diagrams

Summary
	This use case follows a consumer applying through two loans, a few weeks apart, through a fintech lender. The consent terms for each loan application are different, and when the second loan application is denied, the consumer suspects that the lender accessed unauthorized data. The consumer uses MyTrace (a traceability service that uses the OTrace protocol specification) to investigate.

Setup
	Data Subject (consumer) S wishes to apply for loan L from Fintech F. If they don’t already use it, S introduces themself to MyTrace, and introduces F to MyTrace too as the traceability service that F should use when handling personal data about S. For this loan product, F requires the past three months of their transaction data from Bank B, along with their utility payment data from the past year from Utility U. F requests consent with terms providing that this data is to be used only for the loan offer decisioning on loan L, with an expiry of 1 month. Once Data Subject (consumer) S accepts the consent terms, S authorizes B and U to share data with F.

F then uses this data per the consent to make a loan eligibility determination and approve S for the loan. Using MyTrace (/the OTrace protocol), S can then use filter criteria to search (generic concept) through attestations about uses of their data to find the uses of data from B and U for the loan determination, identify the consents that served as a basis for this use (per the data use concept), and check (generic concept) that these consents were valid.

3 weeks later, Data Subject (consumer) S misses a utility payment and applies for a different loan M from Fintech F, this time only giving consent to use transaction data from B. After going through a similar consent and authorization process, their loan application is denied. They are concerned F used data from U to determine credit eligibility for M, so they once again search through past attestations to find data used for the loan M eligibility decision, identify the consents that were the basis for this use, and check their validity.

Notation Guide

concept.action(argument: value) → output - This indicates that action has been called by a 
party and output was returned..

Introduction
Presumably, F has the same identity within the TraceService MyTrace, across all its subjects that use MyTrace. At some point, when engineers at F built up infrastructure to interface with the MyTrace API, F would have “introduced themself” to MyTrace. The same is true for B and U. So at some point in the past, this happened:

introduction.introduce(subject: F, controller: F, traceService: MyTrace) → F_F_introduction

attestation.attest(party: F, context: {“initial_introduction”: F_F_introduction}, body: “F introduces themself to MyTrace”) → att_1

introduction.introduce(subject: B, controller: B, traceService: MyTrace) → B_B_introduction

attestation.attest(party: B, context: {“initial_introduction”: B_B_introduction}, body: “B introduces themself to MyTrace”) → att_2

introduction.introduce(subject: U, controller: U, traceService: MyTrace) → U_U_introduction

attestation.attest(party: U, context: {“initial_introduction”: U_U_introduction}, body: “U introduces themself to MyTrace”) → att_3

S also “introduces themself” to MyTrace. This is what happens when they make an account and decide to use MyTrace to trace their personal data.

introduction.introduce(subject: S, controller: S, traceService: MyTrace) → S_S_introduction

attestation.attest(party: S, context: {“initial_introduction”: S_S_introduction}, body: “S introduces themself to MyTrace”) → att_4

S then introduces F to MyTrace, meaning that F now must use MyTrace when handling data about S. F must attest agreement to this.
Note that B and U do not need to be introduced to MyTrace as they do not make any attestations in this scenario; they only provide data for F to use.

introduction.introduce(subject: S, controller: F, traceService: MyTrace) → S_F_introduction

attestation.attest(party: S, context: {“subject”: att_4, “controller”: att_1}, body: “S introduces F to MyTrace”) → att_5
attestation.attest(party: F, context: {“subject”: att_4, “controller”: att_1, “introduction”: att_5}, body: “F agrees to use MyTrace for data about S”) → att_6


Loan L
Consent Process
Note that we have separate consents for F to use data from B and U. This gives consumers more granular consent, revocation power, and insight into which providers have which data and what they do with it, in line with the results of our user study.

Time.now() → T1
consent.request(controller: F, subject: S, terms: use 3mo transaction data from B in credit 
decisioning for loan L, expiration: T1+1mo) → c_B_L_request
consent.request(controller: F, subject: S, terms: use 12mo payments data from U in credit 
decisioning for loan L, expiration: T1+1mo) → c_U_L_request

attestation.attest(party: F, context: {“subject”: att_4, “controller”: att_1}, body: “F requests consent from S with terms ‘use 3mo transaction data from B in credit decisioning for loan L’ and expiration ‘T1+1mo’”) → att_7
attestation.attest(party: F, context: {“subject”: att_4, “controller”: att_1}, body: “F requests consent from S with terms ‘use 12mo payments data from U in credit decisioning for loan L’ and expiration ‘T1+1mo’”) → att_8

consent.accept(subject: S, consent: c_B_L_request) → c_B_L_accepted
consent.accept(subject: S, consent: c_U_L_request) → c_U_L_accepted

attestation.attest(subject: S, context: {“subject”: att_4, “controller”: att_1, “consent”: att_7}, body: “S accepts consent from F with terms ‘use 3mo transaction data from B in credit decisioning for loan L’ and expiration ‘T1+1mo’”) → att_9
attestation.attest(subject: S, context: {“subject”: att_4, “controller”: att_1, “consent”: att_8}, body: “S accepts consent from F with terms ‘use 12mo transaction data from U in credit decisioning for loan L’ and expiration ‘T1+1mo’”) → att_10


Authorization
Once a data subject (consumer) accepts a consent that involves data from a third party, an authorization should automatically happen, which allows that data transfer to occur. In financial applications, this is typically mediated by the FDX API. In concept-land, we can just represent it with a generic concept and action authorization.authorize(data subject, data provider, data recipient, data, expiration). In this simple example, B and U have not been introduced to MyTrace and have made no relevant consent agreements, so they don’t have to attest to anything for the authorization to occur.

authorization.authorize(subject: S, provider: B, recipient: F, data: 3mo transaction data, 
expiration: T1+1mo) → B_L_authorize
authorization.authorize(subject: S, provider: U, recipient: F, data: 12mo payments data, 
expiration: T1+1mo) → U_L_authorize

attestation.attest(party: S, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “consent”: att_9}, body: “S authorizes 3mo transaction data from B to be sent to F according to the consent agreed to at att_9”) → att_11
attestation.attest(party: S, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_3, “consent”: att_10}, body: “S authorizes 12mo payments data from U to be sent to F according to the consent agreed to at att_10”) → att_12

The recipients and provider must also attest to these authorizations:

attestation.attest(party: B, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “authorization”: att_11}, body: “B agrees to authorization from S at att_11 to provide 3mo transaction data to F”) → att_13
attestation.attest(party: F, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “authorization”: att_11}, body: “F acknowledges authorization from S at att_11 for 3mo transaction data from B to be sent to F”) → att_14
attestation.attest(party: U, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_3, “authorization”: att_12}, body: “U agrees to authorization from S at att_12 to provide 12mo payments data to F”) → att_15
attestation.attest(party: F, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_3, “authorization”: att_12}, body: “F acknowledges authorization from S at att_12 for 12mo payments data from U to be sent to F”) → att_16



Data Use
Now, Fintech F is going to use the dataUse concept to declare that it used data and associate the uses with bases--in this case, consents.

dataUse.use(controller: F, data subject: S, data: 3mo transaction data from B, operation: credit 
decisioning for loan L, basis: consent c_B_L_accepted) → use_B_L
dataUse.use(controller: F, data subject: S, data: 12mo payments data from U, operation: credit 
decisioning for loan L, basis: consent c_U_L_accepted) → use_U_L

attestation.attest(party: F, context: {“subject”: att_4, “data”: att_11, “basis”: att_9)}, body: “F uses data from subject S given at authorization att_11 according to basis att_9”) → att_17
attestation.attest(party: F, context: {“subject”: att_4, “data”: att_12, “basis”: att_10)}, body: “F uses data from subject S given at authorization att_12 according to basis att_10”) → att_18


The Consumer Investigates with MyTrace/OTrace Protocol
The consumer’s three basic queries are SHOW! (to ask what attestations exist), WHY? (to see the legal bases of any attestations about data use), and OK? (to check whether the data use was legal under the basis at the time).
Note that whenever the consumer asks SHOW!, synchronizations trigger the fintech to assert that all of its attestations are up to date by listing all of the attestation IDs from the creation of the first consent returned in the results--in this case, att_c_B_L_offer.

search.search(entries: attestation.getAll(), query: “attestations by party F about data subject 
S”) → [att_6, att_7, att_8, att_14, att_16, att_17, att_18]
	^(consumer asks SHOW!)

dataUse.getBasis(dataUse: att_17) → att_9
dataUse.getBasis(dataUse: att_18) → att_10
	^(consumer asks WHY?)

check.check(action: att_17, basis: att_9) → True
check.check(action: att_18, basis: att_10) → True
	^(consumer asks OK?)

Loan M
Consent Process
In this case, we only have one consent, as there is only one data source.
time.now() → T2 (= T1 + 3 weeks)
consent.request(controller: F, subject: S, terms: use 3mo transaction data from B in credit 
decisioning for loan M, expiration: T2+1mo) → c_B_M_request

attestation.attest(party: F, context: {“subject”: att_4, “controller”: att_1}, body: “F requests consent from S with terms ‘use 3mo transaction data from B in credit decisioning for loan M’ and expiration ‘T2+1mo’”) → att_19

consent.accept(subject: S, consent: c_B_M_request) → c_B_M_accepted

attestation.attest(subject: S, context: {“subject”: att_4, “controller”: att_1, “consent”: att_19}, body: “S accepts consent from F with terms ‘use 3mo transaction data from B in credit decisioning for loan M’ and expiration ‘T2+1mo’”) → att_20


Authorization
Similarly, there is only one authorization, which is once again automatic.

authorization.authorize(subject: S, provider: B, recipient: F, data: 3mo transaction data, 
expiration: T1+1mo) → B_M_authorize

attestation.attest(party: S, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “consent”: att_20}, body: “S authorizes 3mo transaction data from B to be sent to F according to the consent agreed to at att_20”) → att_21


attestation.attest(party: B, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “authorization”: att_21}, body: “B agrees to authorization from S at att_21 to provide 3mo transaction data to F”) → att_22
attestation.attest(party: F, context: {“subject”: att_4, “recipient”: att_1, “provider”: att_2, “authorization”: att_21}, body: “F acknowledges authorization from S at att_21 for 3mo transaction data from B to be sent to F”) → att_23


Version 1: Provider Attests to Wrong Consent
From here, if F is indeed wrongfully using data from U to make an eligibility decision about Loan M, they could attest to it or not. In version 1, they attest to using the data under the incorrect consent agreement. Here, red text is used for records of this incorrect consent and associated attestations.

Data Use
dataUse.use(controller: F, data subject: S, data: 3mo transaction data from B, operation: credit 
decisioning for loan M, basis: consent c_B_M_accepted) → use_B_M
dataUse.use(controller: F, data subject: S, data: 12mo payments data from U, operation: credit 
decisioning for loan M, basis: consent c_U_L_accepted) → use_U_M

attestation.attest(party: F, context: {“subject”: att_4, “data”: att_21, “basis”: att_20)}, body: “F uses data from subject S given at authorization att_21 according to basis att_20”) → att_24
attestation.attest(party: F, context: {“subject”: att_4, “data”: att_12, “basis”: att_10)}, body: “F uses data from subject S given at authorization att_12 according to basis att_10”) → att_25

attestation.upToDate(d1: att_19.timestamp, d2: att_24.timestamp, s: [att_19, att_20, att_21, att_22, att_23, att_24])
attestation.upToDate(d1: att_8.timestamp, d2: att_25.timestamp, s: [att_8, att_10, att_12, att_15, att_16, att_18, att_25])


The Consumer Investigates with MyTrace/OTrace Protocol
search.search(entries: attestation.getAll(), query: “attestations by party F about consumer X”) 
→ [att_6, att_7, att_8, att_14, att_16, att_17, att_18, att_19, att_23, att_24, att_25]
^user asks SHOW!

…
dataUse.getBasis(dataUse: att_25) → att_10
	^user asks WHY?
…
check.check(action: att_25, basis: att_10) → False
	^user asks OK?

Version 2: Provider Does Not Attest 
In this version of events, F does not attest to using data from U.

Data Use
dataUse.use(controller: F, data subject: S, data: 3mo transaction data from B, operation: credit 
decisioning for loan M, basis: consent c_B_M_accepted) → use_B_M

attestation.attest(party: F, context: {“subject”: att_4, “data”: att_21, “basis”: att_20)}, body: “F uses data from subject S given at authorization att_21 according to basis att_20”) → att_24

attestation.upToDate(d1: att_19.timestamp, d2: att_24.timestamp, s: [att_19, att_20, att_21, att_22, att_23, att_24])



The Consumer Investigates with MyTrace

search.search(entries: attestation.getAll(), query: “attestations by party F about consumer X”) 
→ [att_6, att_7, att_8, att_14, att_16, att_17, att_18, att_19, att_23, att_24]
^user asks SHOW!

In this scenario, no faulty attestation is returned. So if a company is not complying with the traceability protocol, then it falls to out-of-band methods (e.g. external audit) to determine the violation.

NOTE: This use case was crafted BEFORE the design of the TraceAgent idea. Now, we want a configurable TraceAgent to automatically detect issues that the consumer should know about and notify them accordingly.



Here are the attestations for this use case in a JSON list (these would be stored in a server)

[
  {
    "timestamp": "01/11/2025 2:28:00",
    "id": 1,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.lendingclub.com/"
    },
    "body": "Identification created for LendingClub"
  },
  {
    "timestamp": "01/18/2025 7:05:00",
    "id": 2,
    "party": "CitiBank",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.citi.com/"
    },
    "body": "Identification created for CitiBank"
  },
  {
    "timestamp": "04/03/2025 15:52:00",
    "id": 3,
    "party": "Eversource",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.eversource.com/"
    },
    "body": "Identification created for Eversource"
  },
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
    "timestamp": "07/01/2025 22:02:00",
    "id": 5,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-introduce",
      "controller": 1,
      "subject": 4
    },
    "body": "Jane Smith introduces LendingClub to MyTrace"
  },
  {
    "timestamp": "07/01/2025 22:03:00",
    "id": 6,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#introduction-acknowledge",
      "controller": 1,
      "introduction": 5,
      "subject": 4
    },
    "body": "LendingClub agrees to use MyTrace for data about Jane Smith"
  },
  {
    "timestamp": "07/01/2025 22:28:00",
    "id": 7,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=user.financial.bank_account",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 2,
      "subject": 4,
      "start": "04/01/2025 22:28:00",
      "end": "07/01/2025 22:28:00"
    },
    "body": "LendingClub defines a dataset of 3mo transaction data from Citibank."
  },
  {
    "timestamp": "07/01/2025 22:28:01",
    "id": 8,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 7,
      "purpose": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "purposeOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub defines term to use 3mo transaction data from Citibank in credit decisioning"
  },
  {
    "timestamp": "07/01/2025 22:28:00",
    "id": 9,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=configuration%20and%20setting.-,user.payment,-user",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 3,
      "subject": 4,
      "start": "07/01/2024 22:28:00",
      "end": "07/01/2025 22:28:00"
    },
    "body": "LendingClub defines a dataset of 12mo payments data from Eversource."
  },
  {
    "timestamp": "07/01/2025 22:28:01",
    "id": 10,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 9,
      "purpose": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "purposeOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub defines term to use 12mo payments data from Eversource in credit decisioning"
  },
  {
    "timestamp": "07/01/2025 22:28:02",
    "id": 11,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request",
      "controller": 1,
      "subject": 4,
      "terms": [8,10],
      "expiry": "08/01/2025 00:00:00"
    },
    "body": "LendingClub requests consent from Jane Smith with terms to use 3mo transactions data from Citibank and 12mo utility payments data from Eversource in credit decisioning. It will expire on August 1, 2025"
  },
  {
    "timestamp": "07/01/2025 22:29:00",
    "id": 12,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-accept",
      "consent": 11,
      "controller": 1,
      "subject": 4
    },
    "body": "Jane Smith accepts consent from LendingCLub"
  },
  {
    "timestamp": "07/01/2025 22:29:00",
    "id": 13,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize",
      "consent": 11,
      "recipient": 1,
      "provider": 2,
      "subject": 4,
      "protocol": "https://developer.financialdataexchange.org/fdx-api-v6-2-0",
      "data": 7,
      "expiry": "08/01/2025 00:00:00"
    },
    "body": "Jane Smith authorizes 3mo transaction data from Citibank to be sent to LendingCLub according to the consent"
  },
  {
    "timestamp": "07/01/2025 22:29:00",
    "id": 14,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize",
      "consent": 11,
      "recipient": 1,
      "provider": 3,
      "subject": 4,
      "protocol": "https://developer.financialdataexchange.org/fdx-api-v6-2-0",
      "data": 9,
      "expiry": "08/01/2025 00:00:00"
    },
    "body": "Jane Smith authorizes 12mo payments data from Eversource to be sent to LendingCLub according to the consent"
  },
  {
    "timestamp": "07/01/2025 23:55:00",
    "id": 15,
    "party": "Citibank",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant",
      "dataAuthorization": 13
    },
    "body": "Citibank provides LendingClub access to 3mo transaction data"
  },
  {
    "timestamp": "07/02/2025 9:00:00",
    "id": 16,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access",
      "dataAuthorization": 13
    },
    "body": "LendingClub pulls 3mo transaction data from Citibank"
  },
  {
    "timestamp": "07/04/2025 10:37:00",
    "id": 17,
    "party": "Eversource",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant",
      "dataAuthorization": 14
    },
    "body": "Eversource provides LendingClub access to 12mo payments data"
  },
  {
    "timestamp": "07/04/2025 15:52:00",
    "id": 18,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access",
      "dataAuthorization": 14
    },
    "body": "LendingClub pulls 12mo payments data from Eversource"
  },
  {
    "timestamp": "07/06/2025 10:43:00",
    "id": 19,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 12,
      "data": 7,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub uses data from subject according to basis"
  },
  {
    "timestamp": "07/06/2025 10:43:00",
    "id": 20,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 12,
      "data": 9,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub uses data from subject according to basis"
  },
  {
    "timestamp": "07/06/2025 10:43:00",
    "id": 21,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create",
      "subject": 4,
      "dataUses": [19,20]
    },
    "body": "offered you a loan"
  },
  {
    "timestamp": "07/07/2025 12:40:00",
    "id": 22,
    "party": "Ally Financial",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#identification-create",
      "identifier": "https://www.ally.com/"
    },
    "body": "Identification created for Ally Financial"
  },
  {
    "timestamp": "07/08/2025 22:02:00",
    "id": 23,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=user.financial.bank_account",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 2,
      "subject": 4,
      "start": "04/08/2025 22:02:00",
      "end": "07/08/2025 22:02:00"
    },
    "body": "LendingClub defines a dataset of 3mo transactions data from Citibank."
  },
  {
    "timestamp": "07/08/2025 22:02:01",
    "id": 24,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 23,
      "purpose": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "purposeOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub defines term to use 3mo transaction data from Citibank in credit decisioning"
  },
  {
    "timestamp": "07/08/2025 22:02:00",
    "id": 25,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=configuration%20and%20setting.-,user.payment,-user",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 3,
      "subject": 4,
      "start": "07/08/2024 22:02:00",
      "end": "07/08/2025 22:02:00"
    },
    "body": "LendingClub defines a dataset of 12mo payments data from Eversource."
  },
  {
    "timestamp": "07/08/2025 22:02:01",
    "id": 26,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 25,
      "purpose": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "purposeOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub defines term to use 12mo payments data from Eversource in credit decisioning"
  },
  {
    "timestamp": "07/08/2025 22:02:00",
    "id": 27,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#data-define",
      "dataType": "https://ethyca.github.io/fideslang/taxonomy/data_categories/#:~:text=configuration%20and%20setting.-,user.payment,-user",
      "dataTypeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_categories/",
      "controller": 22,
      "subject": 4,
      "start": "07/08/2024 22:02:00",
      "end": "07/08/2025 22:02:00"
    },
    "body": "LendingClub defines a dataset of 12mo payments data from Ally Financial."
  },
  {
    "timestamp": "07/08/2025 22:02:01",
    "id": 28,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#term-define",
      "data": 27,
      "purpose": "https://ethyca.github.io/fideslang/taxonomy/data_uses/#:~:text=and%20legitimate%20purposes-,marketing,-%2D",
      "purposeOntology": "https://ethyca.github.io/fideslang/taxonomy/data_uses/"
    },
    "body": "LendingClub defines term to use 12mo payments data from Ally Financial in marketing"
  },
  {
    "timestamp": "07/08/2025 22:02:02",
    "id": 29,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-request",
      "controller": 1,
      "subject": 4,
      "terms": [24,26,28],
      "expiry": "08/08/2025 00:00:00"
    },
    "body": "LendingClub requests consent from Jane Smith to use 3mo bank transaction data from Citibank, 12mo payments data from Eversource, and 12mo payments data from Ally Financial in credit decisioning' and expiration August 8, 2025"
  },
  {
    "timestamp": "07/08/2025 22:05:00",
    "id": 30,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#consent-accept",
      "consent": 29,
      "controller": 1,
      "subject": 4
    },
    "body": "Jane Smith accepts consent from LendingClub"
  },
  {
    "timestamp": "07/08/2025 22:06:00",
    "id": 31,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize",
      "consent": 29,
      "controller": 1,
      "provider": 2,
      "subject": 4,
      "protocol": "https://developer.financialdataexchange.org/fdx-api-v6-2-0",
      "data": 23,
      "expiry": "08/08/2025 00:00:00"
    },
    "body": "Jane Smith authorizes 3mo transaction data from Citibank to be sent to LendingClub according to the consent"
  },
  {
    "timestamp": "07/08/2025 22:06:00",
    "id": 32,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize",
      "consent": 29,
      "controller": 1,
      "provider": 3,
      "subject": 4,
      "protocol": "https://developer.financialdataexchange.org/fdx-api-v6-2-0",
      "data": 25,
      "expiry": "08/08/2025 00:00:00"
    },
    "body": "Jane Smith authorizes 12mo utility payments data from Eversource to be sent to LendingClub according to the consent"
  },
  {
    "timestamp": "07/08/2025 22:06:00",
    "id": 33,
    "party": "Jane Smith",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-authorize",
      "consent": 29,
      "controller": 1,
      "provider": 22,
      "subject": 4,
      "protocol": "https://developer.financialdataexchange.org/fdx-api-v6-2-0",
      "data": 27,
      "expiry": "08/08/2025 00:00:00"
    },
    "body": "Jane Smith authorizes 3mo car payments data from Ally Financial to be sent to LendingClub according to the consent"
  },
  {
    "timestamp": "07/09/2025 09:12:00",
    "id": 34,
    "party": "Citibank",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant",
      "dataAuthorization": 31
    },
    "body": "Citibank provides LendingClub access to 3mo transaction data"
  },
  {
    "timestamp": "07/09/2025 09:14:00",
    "id": 35,
    "party": "Eversource",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant",
      "dataAuthorization": 32
    },
    "body": "Eversource provides LendingClub access to 12mo payments data"
  },
  {
    "timestamp": "07/09/2025 09:15:00",
    "id": 36,
    "party": "Ally Financial",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-grant",
      "dataAuthorization": 33
    },
    "body": "Ally Financial provides LendingClub access to 12mo payments data"
  },
  {
    "timestamp": "07/09/2025 10:00:00",
    "id": 37,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access",
      "dataAuthorization": 31
    },
    "body": "LendingClub pulls 3mo transaction data from Citibank"
  },
  {
    "timestamp": "07/09/2025 10:01:00",
    "id": 38,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access",
      "dataAuthorization": 32
    },
    "body": "LendingClub pulls 12mo payments data from Eversource"
  },
  {
    "timestamp": "07/09/2025 10:02:00",
    "id": 39,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataAuthorization-access",
      "dataAuthorization": 33
    },
    "body": "LendingClub pulls 12mo payments data from Ally Financial"
  },
  {
    "timestamp": "07/10/2025 14:42:00",
    "id": 40,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 30,
      "data": 23,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub uses bank transaction data from Citibank to make a credit decision"
  },
  {
    "timestamp": "07/10/2025 14:43:00",
    "id": 41,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 30,
      "data": 25,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub uses utility payments data from Eversource to make a credit decision"
  },
  {
    "timestamp": "07/10/2025 14:44:00",
    "id": 42,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataUse-use",
      "basis": 30,
      "data": 27,
      "subject": 4,
      "operation": "https://nicolatl.github.io/mytrace-web/ontology#credit-decisioning",
      "operationOntology": "https://nicolatl.github.io/mytrace-web/ontology"
    },
    "body": "LendingClub uses car payment data from Ally Financial to make a credit decision"
  },
  {
    "timestamp": "07/10/2025 14:45:00",
    "id": 43,
    "party": "LendingClub",
    "context": {
      "type": "https://nicolatl.github.io/mytrace-web/docs/1.0#dataReceipt-create",
      "subject": 4,
      "dataUses": [40,41,42]
    },
    "body": "denied your loan application"
  }
]
