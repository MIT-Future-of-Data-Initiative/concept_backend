concept disclosure [Data, Provider, Recipient, Subject]
purpose represents a provider sharing data about a subject with a recipient (initiated by 
provider)
state 
subject: Disclosure → Subject
provider: Disclosure → Provider
recipient Disclosure: → Recipient
data: Disclosure → set Data
expiration: Disclosure → Timestamp
consent: Disclosure → Consent
protocol: Disclosure → Protocol
actions
// called by provider
grant(s: Subject, p: Provider, r: Recipient, dd: set Data, e: Expiration, c: Consent, pr: 
Protocol) → d: Disclosure
creates fresh disclosure d such that p is provided access to dd
		d.subject = s
		d.provider = p
		d.recipient = r
		d.data = dd
		d.expiration = e
		d.consent = c
		d.protocol = p
	revoke(d: Disclosure) → d: Disclosure
		d.expiration = Timestamp.now()
	// called by controller
access(d: Disclosure) 
	d.recipient pulls d.data from d.provider.
operational principle
	after grant(s,p,r,dd,e,c,pr) and until revoke(s,a) or Time.now >= e, p shares d with r.