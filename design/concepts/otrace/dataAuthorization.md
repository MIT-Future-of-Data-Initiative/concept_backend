concept dataAuthorization [Data, Provider, Recipient, Subject]
purpose to authorize a provider to share data about a subject with a recipient (initiated by subject)
state 
subject: DataAuthorization → Subject
provider: DataAuthorization → Provider
recipient: DataAuthorization → Recipient
data: DataAuthorization → set Data
expiration: DataAuthorization → Timestamp
consent: DataAuthorization → Consent
protocol: DataAuthorization → Protocol
actions
// called by subject
authorize(s: Subject, p: Provider, r: Recipient, d: set Data, e: Expiration, c: Consent, pr: 
Protocol) → a: DataAuthorization
creates fresh dataAuthorization a such that
		a.subject = s
		a.provider = p
		a.recipient = r
		a.data = d
		a.expiration = e
		a.consent = c
		a.protocol = p
	revoke(s: Subject, a: DataAuthorization) → a: DataAuthorization
		a.expiration = Timestamp.now()
	// called by controller
grant(a: DataAuthorization) 
	a.provider provides a.recipient access to a.data.
access(a: DataAuthorization) 
	a.recipient pulls a.data from a.provider.
operational principle
	after authorize(s,p,r,d,e,c,pr) and grant(a,d), and until revoke(s,a) or Time.now >= e, p must share d with r.