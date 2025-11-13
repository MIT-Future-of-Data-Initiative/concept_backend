concept consent [Controller, Data, Subject, Terms]
purpose to form and manage consent agreements governing a controller’s use of subject’s personal data
state
// type definitions
Term: (Data, Purpose)
ConsentStatus: REQUESTED | ACCEPTED | DENIED | REVOKED | EXPIRED
// fields
controller: Consent → Controller
subject: Consent → Subject
terms: Consent → set Term
expiry: Consent → Timestamp
status: Consent → ConsentStatus
actions
	// called by controller
request(c: Controller, s: Subject, ts: set Term, e: Timestamp) → consent: Consent
	creates fresh consent such that
		consent.controller = c
		consent.subject = s
		consent.terms = ts
		consent.expiry = e
		consent.status = REQUESTED
	// called by subject
accept(s: Subject, c: Consent) → c: Consent
	requires c.status = REQUESTED
	c.status = ACCEPTED
deny(s: Subject, c: Consent) → c: Consent
	requires c.status = REQUESTED and c.subject = s
	c.status = DENIED
revoke(s: Subject, c: Consent) → c: Consent
	requires c.status = ACCEPTED and c.subject = s
		c.status = REVOKED
	// background action
expire(c: Consent)
	requires c.status = ACCEPTED and c.expiry is before now
		c.status = EXPIRED
	// called by controller
	permit(c: Controller, s: Subject, t: Term)
		requires some consent c where c.status = ACCEPTED and t in c.terms


operational principle
// after a controller requests consent for use of a subject’s data under some terms, and the subject accepts that consent, then so long as the subject does not revoke the consent and the consent does not expire, the controller is permitted to use the subject’s data on those terms
