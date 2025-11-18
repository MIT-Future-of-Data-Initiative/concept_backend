concept attestation [Action, Attestor, DataSubject]  
purpose to allow parties to attest to report actions performed at a particular time on data about a specific data subject
state  
// fields  
attestor: Attestation → Party  
dataSubject: Attestation → DataSubject  // possibly set[DataSubjects], all “parties” associated with a consent  
action: Attestation → Action  
timestamp: Attestation → Timestamp  
// global state
attestations: set Attestation
actions
	// called by party
attest(at: Attestor, a: Action, ds: Data Subject)
	creates fresh attestation att such that
	att.party = p
	att.action = a
	att.timestamp = Timestamp.now()
	attestations.add(att)
// called by subject
getAll(ds: DataSubject) → attestations: set Attestation

// called by controller
upToDate(startDate: Date, endDate: Date, as: set Attestation)
generate an object attesting that as includes all actions between startDate and endDate

operational principle
	after attest(p, a) → att, then att in getAll()
